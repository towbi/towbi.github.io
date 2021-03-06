---
layout: post
title: Name-based (hashing) UUIDs using SHA-256 in Java
lang: en
keywords: "Java, UUID, 5, 3, name-based, name, namespace, namespaced, hash, hashing, SHA, SHA-256"
---

RFC 4122 requires the use of MD5 oder SHA-1 to generate name-based UUIDs, prefering SHA-1 when possible. Since the standard has not been updated since more than a decade, more robust hash functions are not included in the standard. SHA-256 for example has a better resistance against preimage and collision attacks than SHA-1 and should retain these properties even after truncation. For that reason I developed the following class using SHA-256 to create the message digest, instead of MD5 or SHA-1.

{% highlight java %}
import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;
import java.util.Arrays;
import java.util.Objects;
import java.util.UUID;
 
/**
 * Class to create name-based UUIDs using SHA-256 as hashing function.
 * <p>A lot of code in here is taken from {@link UUID}.</p>
 */
public class UUIDSha256 {
 
    /**
     * Static factory to retrieve a name-based (hashing) {@link UUID} based on
     * the specified namespace and name.
     * <p>This method corresponds to {@link UUID#nameUUIDFromBytes(byte[])}</p>
     *
     * @param namespace namespace for the UUID
     * @param name byte array
     * @return A {@link UUID} generated from the specified namespace and name
     */
    public static UUID nameUUIDFromNamespaceAndBytes(UUID namespace, byte[] name) {
        Objects.requireNonNull(namespace, "namespace is null");
        Objects.requireNonNull(name, "name is null");
 
        MessageDigest md;
        try {
            md = MessageDigest.getInstance("SHA-256");
        } catch (NoSuchAlgorithmException nsae) {
            throw new InternalError("SHA-256 not supported");
        }
 
        md.update(toBytes(namespace));
        md.update(name);
        byte[] sha256Bytes = md.digest();
 
        sha256Bytes[6] &= 0x0f; /* clear version */
        sha256Bytes[6] |= 0x50; /* set to version 5 */
        sha256Bytes[8] &= 0x3f; /* clear variant */
        sha256Bytes[8] |= 0x80; /* set to IETF variant */
 
        return fromBytes(Arrays.copyOfRange(sha256Bytes, 0, 16));
    }
 
    private static UUID fromBytes(byte[] data) {
        long msb = 0;
        long lsb = 0;
 
        assert data.length == 16 : "data must be 16 bytes in length";
 
        for (int i = 0; i < 8; i++) {
            msb = (msb << 8) | (data[i] & 0xff);
        }
 
        for (int i = 8; i < 16; i++) {
            lsb = (lsb << 8) | (data[i] & 0xff);
        }
 
        return new UUID(msb, lsb);
    }
 
    private static byte[] toBytes(UUID uuid) {
        byte[] out = new byte[16];
 
        long msb = uuid.getMostSignificantBits();
        long lsb = uuid.getLeastSignificantBits();
 
        for (int i = 0; i < 8; i++) {
            out[i] = (byte) ((msb >> ((7 - i) * 8)) & 0xff);
        }
 
        for (int i = 8; i < 16; i++) {
            out[i] = (byte) ((lsb >> ((15 - i) * 8)) & 0xff);
        }
 
        return out;
    }
}
{% endhighlight %}
