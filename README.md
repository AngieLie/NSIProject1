package nsiproject1;
import javax.crypto.Cipher;
import java.security.NoSuchAlgorithmException;
import javax.crypto.NoSuchPaddingException;
import javax.crypto.KeyGenerator;
import java.security.Key;
import java.security.InvalidKeyException;
import javax.crypto.IllegalBlockSizeException;
import javax.crypto.BadPaddingException;
import javax.crypto.spec.IvParameterSpec;
import java.security.SecureRandom;
import java.util.Base64;
import java.security.InvalidAlgorithmParameterException;
import java.awt.image.BufferedImage;
import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;
import java.io.InputStream;
import java.util.logging.Level;
import java.util.logging.Logger;
import javax.imageio.ImageIO;
import javax.swing.JFileChooser;
import javax.swing.filechooser.FileNameExtensionFilter;



/**
 *
 * @author Angeline Setiawan
 */
public class NSIProject1 {

public static File promptForFile(String msg)

    {
        JFileChooser chooser = new JFileChooser();
        
        // set the title of the dialog box.
        chooser.setDialogTitle("Pick a file");
        
        // Restrict the file types to JPG, GIF, and PNG files.
        chooser.setFileFilter(new FileNameExtensionFilter(
                "Images", "jpg", "gif", "png"));

        if (chooser.showOpenDialog(null) == JFileChooser.APPROVE_OPTION)
        {
            return chooser.getSelectedFile();
        }
        return null;
    }
    private BufferedImage img;
    
    //public Image(File file) throws IOException{
        //img = ImageIO.read(file);}
    public BufferedImage getImage(){
        return img;}
    public static byte[] getFile() {

        File f = new File("/home/bridgeit/Desktop/Olympics.jpg");
        InputStream is = null;
        try {
            is = new FileInputStream(f);
        } catch (FileNotFoundException e2) {
            // TODO Auto-generated catch block
            e2.printStackTrace();}
        byte[] content = null;
        try {content = new byte[is.available()];
        } catch (IOException e1) {
            // TODO Auto-generated catch block
            e1.printStackTrace();
        }try {is.read(content);} catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();}return content;}


    
public static void main(String[] args) throws
        NoSuchAlgorithmException, NoSuchPaddingException,
        InvalidKeyException, IllegalBlockSizeException,
        BadPaddingException, InvalidAlgorithmParameterException
    {
        String msg;         // The message to encrypt.
        Cipher aesCipher;   // The cipher object
        KeyGenerator aesKeyGen;          // The AES keygenerator.
        SecureRandom rand;               // A secure random number generator.
        byte[] rawIV = new byte[16];     // An AES init. vector.
        IvParameterSpec iv;       // The IV parameter for CBC. Different ciphers
                                  // may have different specifications.
        
        // Make sure we have the correct command line arguments
        if (args.length != 1)
        {
            System.out.println("Usage: EncryptionDemo <msg>");
            System.exit(1);
        }
                                  
        // Set up an AES cipher object.
        aesCipher = Cipher.getInstance("AES/CBC/PKCS5Padding");
        
        // Set up an AES cipher object.
        aesCipher = Cipher.getInstance("AES/ECB/PKCS5Padding");

        System.out.print("Generating key . . . ");
        // Get a key generator object and set the key size to 128 bits.
        aesKeyGen = KeyGenerator.getInstance("AES");
        aesKeyGen.init(128);

        // Generate the key.
        Key aesKey = aesKeyGen.generateKey();
        System.out.println("[ DONE ]");

        // Generate the IV for CBC mode.
        System.out.print("Generating IV  . . . ");
        rand = new SecureRandom();
        rand.nextBytes(rawIV);          // Fill array with random bytes.
        iv = new IvParameterSpec(rawIV);
        System.out.println("[ DONE ]");

        // Put the cipher in encrypt mode with the generated key 
        // and IV. The Cipher object will save message bytes given 
        // to it until doFinal is called, once that happens, the internal 
        // state will be reset.
        aesCipher.init(Cipher.ENCRYPT_MODE, aesKey, iv);

        // Encrypt the entire message at once. The doFinal method 
        // will ensure padding is applied before encrypting the message.
        // One could use the update method instead but, that method returns 
        // the blocks that were encrypted with each call. The doFinal method 
        // still needs to be called to handle the partial block.
        byte[] ciphertext = aesCipher.doFinal(args[0].getBytes());

        // Display the relevant Base-64 data to the screen.
        System.out.println();
        System.out.println("Ciphertext: " + 
                Base64.getEncoder().encodeToString(ciphertext));
        System.out.println("       Key: " + 
                Base64.getEncoder().encodeToString(aesKey.getEncoded()));
        System.out.println("        IV: " +
                Base64.getEncoder().encodeToString(rawIV));
        
    }
}

    
