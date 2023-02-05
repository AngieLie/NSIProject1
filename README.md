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
package nsiproject1;

import java.awt.color.ColorSpace;
import java.io.File;
import java.io.FileNotFoundException;
import java.util.Scanner;

/**
 * Convenience class for working with pixel colors. It has a methods for
 * getting the red, green, blue, and alpha (transparency) channels.
 *
 * @author Zach Kissel
 */
 public class PixelColor {
   public final static int PIXEL_RED_CHANNEL = 16;
   public final static int PIXEL_GREEN_CHANNEL = 8;
   public final static int PIXEL_BLUE_CHANNEL = 0;
   public final static int PIXEL_ALPHA_CHANNEL = 24;

   /**
    * Gets the value of the named channel.
    *
    * @param rgb the RGBA pixel value from the image.
    * @param channel The ID of the channel to check. Must be one of:
    * PIXEL_RED_CHANNEL, PIXEL_GREEN_CHANNEL, PIXEL_BULD_CHANNEL, or 
    * PIXEL_ALPHA_CHANNEL.
    * 
    * @return a number between 0 and 255 inclusive.
    */
    public static int getChannel(int rgb, int channel)
    {
      assert (channel == PIXEL_RED_CHANNEL || channel == PIXEL_GREEN_CHANNEL ||
        channel == PIXEL_BLUE_CHANNEL || channel == PIXEL_ALPHA_CHANNEL);

      return ((rgb >> channel) & 0xFF);
    }
   /**
    * Returns the value of the red channel.
    *
    * @param rgb the RGBA pixel value from the image.
    *
    * @return a number between 0 and 255.
    */
   public static int getRedChannel(int rgb)
   {
     return ((rgb >> 16) & 0xFF);
   }

   /**
    * Returns the value of the green channel.
    *
    * @param rgb the RGBA pixel value from the image.
    *
    * @return a number between 0 and 255.
    */
   public static int getGreenChannel(int rgb)
   {
     return ((rgb >> 8) & 0xFF);
   }

   /**
    * Returns the value of the blue channel.
    *
    * @param rgb the RGBA pixel value from the image.
    *
    * @return a number between 0 and 255.
    */
   public static int getBlueChannel(int rgb)
   {
     return (rgb & 0xFF);
   }

   /**
    * Returns the value of the blue channel.
    *
    * @param rgb the RGBA pixel value from the image.
    *
    * @return a number between 0 and 255.
    */
   public static int getAlphaChannel(int rgb)
   {
     return ((rgb >> 24) & 0xFF);
   }

   /**
    * Returns the value in RGBA format.
    *
    * @param red the value of the red channel [0, 255].
    * @param green the value of the green channel [0, 255].
    * @param blue the value of the blue channel [0, 255].
    * @param alpha the value of the alpha channel [0, 255].
    *
    * @return an RGBA encoded value.
    */
    public static int encodeToRGBA(int red, int green, int blue, int alpha)
    {
        return ((alpha << 24) | (red << 16) | (green << 8) | blue);
    }
    
    /**
     * This method gets the 120 Crayola (tm) crayon colors from a file.
     * 
     * @return a  array of crayon colors.
     * @throws java.io.FileNotFoundException
     * 
     */
    public static int[] getPalette () throws FileNotFoundException
    {
        int[] colors = new int[120];         // 120 Crayloa colors.
        File f = new File("palette.txt");
        Scanner in = new Scanner(f);
        String rgbString;
        String[] channels;
        
        // Load all 120 colors.
        for (int i = 0; i < 120; i++)
        {
            rgbString = in.nextLine();
            channels = rgbString.split(",");
            colors[i] = encodeToRGBA(Integer.parseInt(channels[0]), 
                    Integer.parseInt(channels[1]), 
                    Integer.parseInt(channels[2]), 0);
        }
        
        return colors;
    }
    
    /**
     * Converts an RGB value to the HSV (Hue, Saturation, Value) color space. 
     * @param rgb a value in RGB space.
     * @return an array representing the point in HSV space.
     */
    public static double[] toHSV(int rgb)
    {
        double[] hsv = new double[3];
        
        // Convert each channel to a percentage of the total color.
        double redP = getRedChannel(rgb) / 255.0;
        double greenP = getGreenChannel(rgb) / 255.0;
        double blueP = getBlueChannel(rgb) / 255.0;
        
        // Compute the maximum and minimum channel values.
        double colorMax = Math.max(redP, Math.max(greenP, blueP));
        double colorMin = Math.min(redP, Math.min(greenP, blueP));
        
        // Compute the Hue
        if (colorMax - colorMin == 0)
            hsv[0] = 0;
        else if (colorMax == redP)
            hsv[0] = 60.0 *  
                    (int) ((greenP - blueP) / (colorMax - colorMin)) % 6;
        else if (colorMax == greenP)
            hsv[0] = 60.0 * ((blueP - redP) / (colorMax - colorMin) + 2.0);
        else
            hsv[0] = 60.0 * ((redP - greenP) / (colorMax - colorMin) + 4.0);
        
        // Compute the saturation.
        if (colorMax == 0)
            hsv[1] = 0;
        else
            hsv[1] = (colorMax - colorMin) / colorMax;
        
        // Compute the value.
        hsv[2] = colorMax;
        
        return hsv;
    }
    
    /**
     * This method returns a visually corrected color difference between 
     * two color values.
     * @param rgb1 an RGB value.
     * @param rgb2 an RGB value.
     * @return a color difference, values ~2.3 is a just noticeable difference.
     */
    public static double colorDistance(int rgb1, int rgb2)
    {
        double[] hsv1 = toHSV(rgb1);
        double[] hsv2 = toHSV(rgb2);
        
        // These values are on a cone, we need to look at components using trig.
        // This will linearize the coordinates.
        double dh = Math.sin(Math.toRadians(hsv1[0])) * hsv1[1] * hsv1[2] - 
                    Math.sin(Math.toRadians(hsv2[0])) * hsv2[1] * hsv2[2]; 
        double ds = Math.cos(Math.toRadians(hsv1[0])) * hsv1[1] * hsv1[2] - 
                    Math.cos(Math.toRadians(hsv2[0])) * hsv2[1] * hsv2[2]; 
        double dv = hsv2[2] - hsv1[2];

        return Math.sqrt(dh * dh + ds * ds + dv * dv);
    }  
 }

    
