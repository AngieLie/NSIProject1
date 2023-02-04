 * Click nbfs://nbhost/SystemFileSystem/Templates/Classes/Main.java to edit this template
 */
package encryption;

/**
 *
 * @author angie
 */
/**
 * This program demonstrates how to perform decryption of an AES ciphertext 
 * encrypted in CBC mode.
 * 
 * @author Zach Kissel
 */
import javax.crypto.Cipher;
import java.security.NoSuchAlgorithmException;
import javax.crypto.NoSuchPaddingException;
import javax.crypto.spec.SecretKeySpec;
import java.security.InvalidKeyException;
import javax.crypto.IllegalBlockSizeException;
import javax.crypto.BadPaddingException;
import java.util.Base64;
import java.util.Scanner;
import java.security.InvalidAlgorithmParameterException;
import javax.crypto.spec.IvParameterSpec;

public class Encryption {
    public static void main(String[] args) throws
        NoSuchAlgorithmException, NoSuchPaddingException, InvalidKeyException,
        IllegalBlockSizeException, BadPaddingException,
        InvalidAlgorithmParameterException
    {
        String key;         // The Base64 encoded key.
        String ciphertext;  // The Base64 encoded ciphertext.
        String iv;          // The Base65 encoded initialization vector.

        // Set up an AES cipher object.
        Cipher aesCipher = Cipher.getInstance("AES/CBC/PKCS5Padding");

        // Setup the input scanner.
        Scanner input = new Scanner(System.in);

        // Prompt for the ciphertext.
        System.out.print("Please enter ciphertext: ");
        ciphertext = input.nextLine();

        // Prompt for the key.
        System.out.print("Please enter the secret key: ");
        key = input.nextLine();

        // Prompt for the IV.
        System.out.print("Please enter the IV: ");
        iv = input.nextLine();

        System.out.print("\nSetting up cipher . . . ");
        
        // Setup the key.
        SecretKeySpec aesKey = new 
           SecretKeySpec(Base64.getDecoder().decode(key), "AES");

        // Put the cipher in decrypt mode with the specified key.
        aesCipher.init(Cipher.DECRYPT_MODE, aesKey,
            new IvParameterSpec(Base64.getDecoder().decode(iv)));
        System.out.append("[ DONE ]");
        
        // Decrypt the message all in one call.
        byte[] plaintext = aesCipher.doFinal(
                Base64.getDecoder().decode(ciphertext));

        // Display the results.
        System.out.println("\n");
        System.out.println("========== Plaintext (BEGIN) ==========");
        System.out.println(new String(plaintext));
        System.out.println("========== Plaintext (END)   ==========");
    }
}
