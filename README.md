import java.util.Scanner;
import java.io.FileNotFoundException;
import java.io.*;
import java.io.FileWriter;
import java.io.IOException;
import java.util.ArrayList;
import java.util.regex.Pattern;
import java.util.regex.Matcher;

public class SouthieAccent {

	public static void main(String[] args) throws FileNotFoundException {
		// read file using arrays 
    	File movieScript = new File("Jaws.txt");
        if (!movieScript.exists()) {
           System.out.println("File " + movieScript + " does not exist.");
            System.exit(-1);  // indicates unsuccessful termination
        }
        
        // read line by line from file using ArrayList  
        ArrayList<String> readLines = read_Script(movieScript);
        
        
        ArrayList <String> arr_Lines = new ArrayList <String>();
        for (String line : readLines) {
        	arr_Lines.add(southie_Styles(line));  // add southie_Styles in array 
        }

        // write to file
        // getName returns the file name 
        String fileName = movieScript.getName();
        System.out.println(fileName);
        File outputFile = new File(fileName);
        
        try {
        	// create new empty file
            if (outputFile.createNewFile()) {
                FileWriter myWriter = new FileWriter(outputFile.getName());
                for (String line: arr_Lines)
                	myWriter.write(line + "\n");
            } else {
                System.out.println("This File already exists.\n");
                for (String line: arr_Lines) 
                	System.out.println(line);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }

    }

    public static ArrayList<String> read_Script(File n) throws FileNotFoundException {
        Scanner read_Script = new Scanner(n);
        ArrayList <String> l = new ArrayList <String>();
        read_Script.useDelimiter(".");
        while (read_Script.hasNextLine()) {
            String line = read_Script.nextLine();
            l.add(line.replaceAll("\\s+", " "));  // handles spaces
        }
        read_Script.close();
        return l;
    }
    
    public static String replace_very (String s) {
        // replace the word very with word wicked
    	
        Pattern replace_very = Pattern.compile("very", Pattern.CASE_INSENSITIVE);
        Matcher matchVery = replace_very.matcher(s);
        while (matchVery.find()) {
            s = matchVery.replaceAll("wicked");
        }
        return s;
    }
    
    public static String r_vowel(String s) {
        // replace r after a vowel at the end of the word with h 
    	
        Pattern r_vowel = Pattern.compile("[a,e,i,o,u]r[a-z]", Pattern.CASE_INSENSITIVE);
        Matcher mh_rVoEnd = r_vowel.matcher(s);
        while (mh_rVoEnd.find()) {
            int starting = mh_rVoEnd.start();
            char[] arr = s.toCharArray();
            arr[starting + 1] = 'h';
            s = new String(arr);
        }
        return s;
    }
    
    public static String eer_yah_ (String s) {
        // r at the end of word preceded by 'ee' then replace r with yah
        // replace eer to eeyah 
    	
        Pattern eer_yah_ = Pattern.compile("e{2}r\\b", Pattern.CASE_INSENSITIVE);
        Matcher mh_ee_rAtEnd = eer_yah_.matcher(s);
        while (mh_ee_rAtEnd.find()) {
            int r_Atend = mh_ee_rAtEnd.end();
            StringBuilder newStr = new StringBuilder();
            for (int i = 0; i < s.length(); i++) {
                if (i == r_Atend - 1) {
                	newStr.append("yah");
                }
                else {
                	newStr.append(s.charAt(i));
                }
            }
            s = newStr.toString();
        }
        return s;
    }
    
    public static String ir_iyah_ (String s) {
        // r at the end of word preceded by 'i' then replace r with iyah
        // replace ir to iyah 
    	
        Pattern ir_iyah_ = Pattern.compile("ir\\b", Pattern.CASE_INSENSITIVE);
        Matcher mh_ir_rAtEnd = ir_iyah_.matcher(s);
        while (mh_ir_rAtEnd.find()) {
            int r_Atend = mh_ir_rAtEnd.end();
            StringBuilder newStr = new StringBuilder();
            for (int i = 0; i < s.length(); i++) {
                if (i == r_Atend - 1) {
                	newStr.append("yah");
                }
                else {
                	newStr.append(s.charAt(i));
                }
            }
            s = newStr.toString();
        }
        return s;
    }
    
    public static String oor_oowah_ (String s) {
        // r at the end of word preceded by 'oo' then replace r with oowah
        // replace oor to oowah
    	
        Pattern oor_oowah_ = Pattern.compile("o{2}r\\b", Pattern.CASE_INSENSITIVE);
        Matcher mh_oo_rAtEnd = oor_oowah_.matcher(s);
        while (mh_oo_rAtEnd.find()) {
            int r_Atend = mh_oo_rAtEnd.end();
            StringBuilder newStr = new StringBuilder();
            for (int i = 0; i < s.length(); i++) {
                if (i == r_Atend - 1) {
                	newStr.append("wah");
                }
                else {
                	newStr.append(s.charAt(i));
                }
            }
            s = newStr.toString();
        }
        return s;
    }
    
    public static String _rAfterVowel (String s) {
    	// replace r before the vowel between the word with h
    	
        Pattern _rAfterVowel = Pattern.compile("[a,e,i,o,u]r", Pattern.CASE_INSENSITIVE);
        Matcher mh_rVowel = _rAfterVowel.matcher(s);
        while (mh_rVowel.find()) {
            int starting = mh_rVowel.start();
            char[] arr = s.toCharArray();
            arr [starting + 1] = 'h';
            s = new String(arr);
        }
        return s;
    }
   
    public static String a_ar_ (String s) {
        // if a word ends in 'a', append 'r'
        // a at the end becomes 'ar'
    	
        Pattern a_ar_ = Pattern.compile("[a-z]a\\b", Pattern.CASE_INSENSITIVE);
        Matcher mh_r_aAtEnd = a_ar_.matcher(s);
        while (mh_r_aAtEnd.find()) {
            int starting = mh_r_aAtEnd.start();
            int a_AtEnd = mh_r_aAtEnd.end();
            s = mh_r_aAtEnd.replaceAll(s.substring(starting, a_AtEnd - 1) + "ar");
        }

        return s;
    }
    
    // call functions
    public static String southie_Styles(String s) {
    	s = replace_very(s);
    	s = r_vowel(s);
    	s = eer_yah_(s);
    	s = ir_iyah_(s);
    	s = oor_oowah_(s);
    	s = _rAfterVowel(s);
    	s = a_ar_(s);
    	
    	return s;
    }
    
}

