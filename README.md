AOI品質監控
PS AOI CPD Auto Judge
AOI Defect Anomaly Detection
C4A OC Defect自動辨識
BM Defect Auto Judge
AOI生畫像欠陷辨識自動分類

MURA品質監控
Mura 鷹眼計畫
RGB Mura Insp. Auto Judge

AUTO Repair 
RGB Auto Tape Judge
Auto Repair

Repair 品質監控與稽核
RGB Auto Defect Judge
Repair Auto Audit

影像量測
線幅、面積自動量測

創新商業轉型獎

2.報告內容：
請參考以下連結，如果選的是年度最佳企業，報告撰寫就是針對得獎的公司各寫一頁的報告，報告內容大概是得獎內容，使用怎樣的科技…….
Ex:英業達寫一頁、亞東預拌寫一頁、中華電信寫一頁、華南銀行寫一頁、今網智慧科技寫一頁

得獎名單 https://event.hbrtaiwan.com/hbrdx/winners.html
https://event.hbrtaiwan.com/hbrdx/
https://event.hbrtaiwan.com/hbrdx/article.html
https://www.gvm.com.tw/article/115926






<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <system.webServer>
        <handlers>
            <add name="flaskcgi" path="*" verb="*" modules="FastCgiModule" scriptProcessor="C:\ProgramData\Anaconda3\envs\py3.7.3\python.exe|D:\flask_web\IPCAM\wfastcgi.py" resourceType="Unspecified" requireAccess="Script" />
            <add name="phpcgi" path="*.php" verb="*" modules="FastCgiModule" scriptProcessor="C:\xampp\php\php-cgi.exe" resourceType="Unspecified" requireAccess="Script" />
        </handlers>
        <defaultDocument>
            <files>
                <add value="web.py" />
            </files>
        </defaultDocument>
    </system.webServer>
</configuration>
