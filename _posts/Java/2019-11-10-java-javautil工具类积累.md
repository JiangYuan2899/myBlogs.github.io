---
layout: post
title: java工具类整理
category: other
tags: [other]
---
# Base64转PDF、PDF转IMG（使用pdfbox插件）

maven配置文件
```
<!-- https://mvnrepository.com/artifact/org.apache.pdfbox/pdfbox -->
<dependency>
   <groupId>org.apache.pdfbox</groupId>
   <artifactId>pdfbox</artifactId>
   <version>2.0.12</version>
</dependency>
```

```
package com.dhht.wechat.util;

import org.apache.commons.io.FileUtils;
import org.apache.pdfbox.pdmodel.PDDocument;
import org.apache.pdfbox.pdmodel.PDPageTree;
import org.apache.pdfbox.rendering.PDFRenderer;
import sun.misc.BASE64Decoder;
import sun.misc.BASE64Encoder;

import javax.imageio.ImageIO;
import javax.servlet.ServletOutputStream;
import javax.servlet.http.HttpServletResponse;
import java.awt.image.BufferedImage;
import java.io.*;

/**
 * @Author: sh
 * @Description: PDFUtil
 * @Date: 11:35 2019/7/1
 */
public class PDFUtil {

    /**
     * 将base64字符串转换为PDF在显示到页面中
     * @param base64String
     * @param httpServletResponse
     */
    public static void base64StringToPDFToPage(String base64String, HttpServletResponse httpServletResponse){
        BASE64Decoder decoder = new BASE64Decoder();
        ByteArrayOutputStream baos = null;
        ServletOutputStream sos = null;
        try {
            byte[] bytes = decoder.decodeBuffer(base64String);
            baos = new ByteArrayOutputStream();
            baos.write(bytes); //把byte写进输出流里
            if (baos != null) {
                httpServletResponse.setContentType("application/pdf");
                httpServletResponse.setContentLength(baos.size());
                httpServletResponse.setHeader("Expires", "0");
                httpServletResponse.setHeader("Cache-Control", "must-revalidate, post-check=0, pre-check=0");
                httpServletResponse.setHeader("Pragma", "public");
                // 设置打印PDF的文件名
                String fileName = "社保证明文件.pdf";
                fileName = new String(fileName.getBytes("UTF-8"), "ISO-8859-1");
                httpServletResponse.setHeader("Content-Disposition", "filename=" + fileName);
                sos = httpServletResponse.getOutputStream();
                baos.writeTo(sos); //byte输出流写入servlet输出流
                sos.flush();
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                sos.close();
                baos.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }

    /**
     *  将base64编码转换成PDF
     *  @param base64String
     *  1.使用BASE64Decoder对编码的字符串解码成字节数组
     *  2.使用底层输入流ByteArrayInputStream对象从字节数组中获取数据；
     *  3.建立从底层输入流中读取数据的BufferedInputStream缓冲输出流对象；
     *  4.使用BufferedOutputStream和FileOutputSteam输出数据到指定的文件中
     */
    public static void base64StringToPDF(String base64String, String pdfPath/*File file*/){
        File file = new File(pdfPath);// 将原来参数修改为字符串
        BASE64Decoder decoder = new BASE64Decoder();
        BufferedInputStream bin = null;
        FileOutputStream fout = null;
        BufferedOutputStream bout = null;
        try {
            //将base64编码的字符串解码成字节数组
            byte[] bytes = decoder.decodeBuffer(base64String);
            //创建一个将bytes作为其缓冲区的ByteArrayInputStream对象
            ByteArrayInputStream bais = new ByteArrayInputStream(bytes);
            //创建从底层输入流中读取数据的缓冲输入流对象
            bin = new BufferedInputStream(bais);
            //创建到指定文件的输出流
            fout  = new FileOutputStream(file);
            //为文件输出流对接缓冲输出流对象
            bout = new BufferedOutputStream(fout);

            byte[] buffers = new byte[1024];
            int len = bin.read(buffers);
            while(len != -1){
                bout.write(buffers, 0, len);
                len = bin.read(buffers);
            }
            //刷新此输出流并强制写出所有缓冲的输出字节，必须这行代码，否则有可能有问题
            bout.flush();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                bout.close();
                fout.close();
                bin.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }

    /**
     * PDF转换为Base64编码
     * @param file
     * @return
     */
    public static String pdfToBase64(File file) {
        BASE64Encoder encoder = new BASE64Encoder();
        FileInputStream fin =null;
        BufferedInputStream bin =null;
        ByteArrayOutputStream baos = null;
        BufferedOutputStream bout =null;
        try {
            fin = new FileInputStream(file);
            bin = new BufferedInputStream(fin);
            baos = new ByteArrayOutputStream();
            bout = new BufferedOutputStream(baos);
            byte[] buffer = new byte[1024];
            int len = bin.read(buffer);
            while(len != -1){
                bout.write(buffer, 0, len);
                len = bin.read(buffer);
            }
            //刷新此输出流并强制写出所有缓冲的输出字节
            bout.flush();
            byte[] bytes = baos.toByteArray();
            return encoder.encodeBuffer(bytes).trim();

        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                fin.close();
                bin.close();
                baos.close();
                bout.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        return null;
    }

    /**
     * pdf转jpg
     * @param pdfPath
     * @param jpgPath
     */
    public static void pdfToJpg(String pdfPath,String jpgPath){
        long start = System.currentTimeMillis();
        //pdf路径
        InputStream stream = null;
        try {
            stream = new FileInputStream(new File(pdfPath));//URLUtil.getStream(url);
            // 加载解析PDF文件
            PDDocument doc = PDDocument.load(stream);
            PDFRenderer pdfRenderer = new PDFRenderer(doc);
            PDPageTree pages = doc.getPages();
            int pageCount = pages.getCount();
            for (int i = 0; i < pageCount; i++) {
                BufferedImage bim = pdfRenderer.renderImageWithDPI(i, 200);
                ByteArrayOutputStream os = new ByteArrayOutputStream();
                ImageIO.write(bim, "jpg", os);
                byte[] datas = os.toByteArray();
                FileUtils.writeByteArrayToFile(new File(jpgPath),datas);
            }
            long end = System.currentTimeMillis();
            long time = (end - start) / 1000;
            System.out.println("pdf转jpg耗时： {}s"+time);
        }catch (Exception e){

        }

    }

    /**
     * base64转jpg
     * @param val
     * @param pdfFile
     * @param jpgFile
     */
    public static void base64ToJPG(String val,String pdfFile,String jpgFile){
        base64StringToPDF(val,pdfFile);
        pdfToJpg(pdfFile,jpgFile);
    }


}

```