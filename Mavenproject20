package tw.edu;

import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestHeader;
import org.springframework.web.bind.annotation.RestController;
import java.io.File;
import java.io.FileWriter;
import java.io.IOException;

@SpringBootApplication
@RestController
public class Mavenproject20 {

    private static final Logger logger = LogManager.getLogger(Mavenproject20.class);

    public static void main(String[] args) {
        // 固定 Port
        System.setProperty("server.port", "9090");
        
        // 【CTF 機制】在伺服器啟動時，自動生成隱藏的 Flag 檔案
        try {
            File flagFile = new File("flag.txt");
            FileWriter writer = new FileWriter(flagFile);
            // 將你的睡眠時間與學號 412631011 一併包裝成標準格式
            writer.write("FLAG{獻出我的睡眠時間!!");
            writer.close();
            System.out.println("[系統提示] 機密 Flag 檔案已建立於伺服器目錄中。");
        } catch (IOException e) {
            System.out.println("Flag 建立失敗：" + e.getMessage());
        }

        SpringApplication.run(Mavenproject20.class, args);
    }

    @GetMapping("/")
    public String index(@RequestHeader(value = "User-Agent", required = false) String userAgent) {
        
        // 【漏洞核心】觸發點依然保留在這裡
        logger.error("Received request from User-Agent: " + userAgent);
        
        // 網頁不再直接給答案，只給予普通的提示訊息
        return "加油!!!目標：利用 CVE-2021-44228 讀取伺服器上的 flag.txt";
    }
}
