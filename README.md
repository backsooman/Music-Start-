# Music-Start-
music
package music;

import java.io.File;
import java.io.IOException;

import javax.sound.sampled.AudioFormat;
import javax.sound.sampled.AudioInputStream;
import javax.sound.sampled.AudioSystem;
import javax.sound.sampled.DataLine;
import javax.sound.sampled.LineUnavailableException;
import javax.sound.sampled.SourceDataLine;
import javax.sound.sampled.UnsupportedAudioFileException;
class Music {
	public static void playSound()
	{
		File file=new File("C:\\Users\\Sooman\\workspace\\practice\\src\\music\\Taylor Swift - The Story Of Us.wav"); //주소 바꿔야되
		AudioInputStream audioInputStream=null;
		SourceDataLine auline=null;
		try{
			audioInputStream=AudioSystem.getAudioInputStream(file);
		}
		catch(UnsupportedAudioFileException e1){
			e1.printStackTrace();
			return;
		}
		catch(IOException e1){
			e1.printStackTrace();
			return;
		}
		AudioFormat format=audioInputStream.getFormat();
		DataLine.Info info=new DataLine.Info(SourceDataLine.class, format);
		try{
			auline=(SourceDataLine) AudioSystem.getLine(info);
			auline.open(format);
		}
		catch(LineUnavailableException e){
			e.printStackTrace();
		}
		catch(Exception e){
			e.printStackTrace();
			return;
		}
		auline.start();
		
		int nBytesRead=0;
		final int EXTERNAL_BUFFER_SIZE=524288;
		byte[] abData=new byte[EXTERNAL_BUFFER_SIZE];
		
		try{
			while(nBytesRead!=-1){
				nBytesRead=audioInputStream.read(abData,0,abData.length);
				if(nBytesRead>=0)
					auline.write(abData, 0, nBytesRead);
			}
		}
		catch(IOException e){
			e.printStackTrace();
			return;
		}
		finally{
			auline.drain();
			auline.close();
		}
	}
}
