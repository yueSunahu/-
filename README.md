# -package 汉明码;

import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.File;
import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

public class 汉明码2 {

	public static void main(String[] args) throws IOException {
		// TODO 自动生成的方法存根
		//int[] n = input();//用数组n接收数据
		//int[] d=encode(n);//用数组d接收汉明码
		//output(d);//输出数据到ouput文件中
		long start = System.currentTimeMillis();    
		input();
	    long end = System.currentTimeMillis();  
	    System.out.println("运行时间："+(end-start)+"毫秒");
	}
	
	//输出数据
	private static void output(int[] d,String fileName) throws IOException {
		// TODO 自动生成的方法存根
		String str = "D:\\java\\test\\data\\output\\";
        BufferedWriter bufwriter = new BufferedWriter(new FileWriter(str+fileName));
        for (Integer i : d) {
            bufwriter.write(i.toString());
            bufwriter.write(" ");
            bufwriter.flush();
        }
           bufwriter.close();
        
	}
	
	//读入输入
	public static void input() throws FileNotFoundException, IOException {
		String str = "D:\\java\\test\\data\\input\\";
		File dir = new File(str);
        File[] files = dir.listFiles();
        int[] result = null;
        if (files != null) {
            for (int i = 0; i < files.length; i++) {
            	 String fileName = files[i].getName();
                 String pathname = str+fileName;
                
                 BufferedReader bufread = new BufferedReader(new FileReader(pathname));
                 String a =null; 
                 //把数组转换成整数数组
                 while (( a = bufread.readLine())!= null) { 
                	 String[] temp = a.split(" ");//以空格拆分字符串
                	 result = new int[temp.length];//int类型数组
                	 for(int j=0;j<temp.length;j++){
                		 result[j] = Integer.parseInt(temp[j]);//整数数组
                	 }
                }
                int[] d=encode(result);//用数组d接收汉明码
         		output(d,fileName);//输出数据到ouput文件中		 
         		bufread.close();
            }
           
        }
	}

	//获得编码位数
	public static int requirek(int n) {
			int r=1;
			 	while(Math.pow(2, r) -r < n+1) {
			 		r++;
			 	}
			 	return r;
		   }
	
	//建立汉明码
	public static  int[] encode(int[] n2){
			int n=n2.length;
			int k=requirek(n2.length);
		    int dn = n+k;
		    int[] d =new int[dn];
		    //建立新的数组，包含信号位
		    for (int i = 0, j = 0, p = 0; i!= d.length  ;i++){
		        if ((i + 1) == (int)Math.pow(2,p) && p < k){
		            d[i] = 0;
		            p++;
		        }
		        else if(j==n2.length)
		        	break;
		        else if (n2[j] == 0 || n2[j] == 1)
		            d[i] = n2[j++];
		       
		     }
		    
		    //求信号位
		for (int i = 0; i != k;i++){
		        int count = 0 ;
		        int index = (int) Math.pow(2, i);
		        for (int j = index - 1; j < d.length ;j += index)
		            for (int a = 0; a!= index && j < d.length; a++, j++)
		                count ^= d[j];
		        d[index - 1] =count;
		}
		    return d;
	}
}
