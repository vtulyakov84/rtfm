```c#
//
//  Author:     Viktor Tulyakov, vtulyakov84@yandex.ru
//  Date:       17.05.2026
//  License:    Apache 2.0
//

// version: 9.0.2604.2302
using Gurux.DLMS;

// version: 8.4.2503.603
using Gurux.Serial;

// В составе проекта необходимо создать соответствующее пространсво имен
using Gurux.DLMS.Reader;
using Gurux.DLMS.Objects;

class Program
{
    // Инициализация пользовательских параметров для связи
    public static string serialport_name = "COM1";

    public static void Main(string[] args)
    {
        // 1. Создаем клиента для DLMS/COSEM связи
        GXDLMSClient client = new GXDLMSClient();
        client.UseLogicalNameReferencing = true;
        client.InterfaceType = Gurux.DLMS.Enums.InterfaceType.HDLC;
        client.ServerAddress = 1;
        client.ClientAddress = 16;

        // 2. Создаем медиа для связи (например, через COM-порт)
        GXSerial media = new GXSerial(serialport_name);

        // 3. Создаем ридер для чтения данных
        GXDLMSReader reader = new GXDLMSReader(
                client, 
                media
            );

        // 4. Открываем среду передачи данных
        try
        {
            media.Open();

        }
        catch (Exception ex)
        {
            Console.WriteLine("Ошибка при открытии доступа к среде передачи: " + ex.Message);
            return;
        }

        GXReplyData reply = new GXReplyData();

        byte[] data;
        data = client.SNRMRequest();

        if (data != null)
        {
            // Send SNRM request and read reply.
            reader.ReadDLMSPacket(data, reply);

            // Has server accepted client.
            client.ParseUAResponse(reply.Data);

            // ---------- Реальные negotiated параметры ----------
            // Пока сомнительно, что эти параметры отличаются от тех, что были запрошены в SNRM запросе.
            Console.WriteLine();
            Console.WriteLine("Согласованные параметры HDLC:");
            Console.WriteLine($"MaxInfoTX  : {client.HdlcSettings.MaxInfoTX}");
            Console.WriteLine($"MaxInfoRX  : {client.HdlcSettings.MaxInfoRX}");
            Console.WriteLine($"WindowTX   : {client.HdlcSettings.WindowSizeTX}");
            Console.WriteLine($"WindowRX   : {client.HdlcSettings.WindowSizeRX}");
        }

        // Generate AARQ request.
        // Split requests to multiple packets if needed. 
        // If password is used all data might not fit to one packet.
        foreach (byte[] it in client.AARQRequest())
        {
            reply.Clear();
            reader.ReadDLMSPacket(it, reply);
        }

        //Parse reply.
        client.ParseAAREResponse(reply.Data);

        // N. Read Association.
        reply.Clear();
        reader.ReadDataBlock(client.GetObjectsRequest(), reply);
        GXDLMSObjectCollection objects = client.ParseObjects(reply.Data, true);


        // N+1. Close connection
        if (media != null && client != null)
        {
            try
            {
                reply.Clear();
                reader.ReadDLMSPacket(client.DisconnectRequest(), reply);
                media.Close();
            }
            catch
            {
                //Ignore if close fails.
            }
            media = null;
            client = null;
        }
    }
}
```