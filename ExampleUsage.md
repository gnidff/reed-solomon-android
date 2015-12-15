# Introduction #

The class EncoderDecoder can be used encode data using  Reed-Solomon forward error correction codes and to repair and decode data that was encoded with Reed-Solomon codes.

Reed-Solomon codes are useful for sending data over lossy connections like the internet, satellites, terrestrial radios and other lossy RF connections.

# Encoding/Decoding Example #

```

EncoderDecoder encoderDecoder = new EncoderDecoder();

try {

String message = new String("EncoderDecoder Example");

byte[] data = message.getBytes();

byte[] encodedData = encoderDecoder.encodeData(data, 5);

System.out.println(String.format("Message: %s", Util.toHex(data)));
System.out.println(String.format("Encoded Message: %s", Util.toHex(encodedData)));

encodedData[0] = (byte)(Integer.MAX_VALUE & 0xFF); // Intentionally screw up the first 2 bytes
encodedData[1] = (byte)(Integer.MAX_VALUE & 0xFF);

System.out.println(String.format("Flawed Encoded Message: %s", Util.toHex(encodedData)));

byte[] decodedData = encoderDecoder.decodeData(encodedData, 5);

System.out.println(String.format("Decoded/Repaired Message: %s", Util.toHex(decodedData)));


} catch (DataTooLargeException e) {
e.printStackTrace();
} catch (ReedSolomonException e) {
e.printStackTrace();
}

```

Output:

Message:                               456E636F64 65724465 636F6465 72204578 616D706C 65

Encoded Message:                 456E636F64 65724465 636F6465 72204578 616D706C 6560693B 4717

Flawed Encoded Message:     FFFF636F64 65724465 636F6465 72204578 616D706C 6560693B 4717

Decoded/Repaired Message: 456E636F64 65724465 636F6465 72204578 616D706C 65