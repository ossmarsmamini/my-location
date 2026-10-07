<!DOCTYPE html>  
<html lang="th">  
<head>  
    <meta charset="UTF-8">  
    <meta name="viewport" content="width=device-width, initial-scale=1.0">  
    <title>ระบบแชร์ตำแหน่งจำกัดเวลา</title>  
    <style>  
        body { font-family: sans-serif; text-align: center; padding: 40px; background: #f4f4f9; margin: 0; }  
        .card { background: white; padding: 30px; border-radius: 12px; box-shadow: 0 4px 15px rgba(0,0,0,0.1); display: inline-block; max-width: 400px; width: 100%; }  
        .active { color: #2e7d32; font-weight: bold; }  
        .expired { color: #c62828; font-weight: bold; }  
        .btn { display: inline-block; margin-top: 15px; padding: 12px 20px; background: #1976d2; color: white; text-decoration: none; border-radius: 6px; font-weight: bold; }  
    </style>  
</head>  
<body>  
    <div class="card">  
        <h2>ระบบแชร์ตำแหน่ง</h2>  
        <div id="status" style="margin: 15px 0;">กำลังตรวจสอบเวลา...</div>  
        <div id="location-info"></div>  
    </div>  
  
    <script>  
        // --- ตั้งค่าช่วงเวลาที่อนุญาตให้ดูตำแหน่ง (ปี-เดือน-วันTเวลา:นาที:วินาที) ---  
        const startTime = new Date('2026-10-06T18:00:00').getTime();  
        const endTime = new Date('2026-10-06T23:59:59').getTime();  
          
        const now = new Date().getTime();  
        const statusDiv = document.getElementById('status');  
        const infoDiv = document.getElementById('location-info');  
  
        if (now >= startTime && now <= endTime) {  
            statusDiv.innerHTML = '<span class="active">สถานะ: เปิดใช้งาน (อยู่ในช่วงเวลาที่กำหนด)</span>';  
              
            // ตรวจสอบและดึงพิกัด GPS  
            if (navigator.geolocation) {  
                navigator.geolocation.getCurrentPosition(function(position) {  
                    const lat = position.coords.latitude;  
                    const lng = position.coords.longitude;  
                    infoDiv.innerHTML = `  
                        <p>พิกัดของคุณถูกบันทึกเรียบร้อย</p>  
                        <a href="https://maps.google.com/?q=${lat},${lng}" target="_blank" class="btn">เปิดตำแหน่งใน Google Maps</a>  
                    `;  
                }, function(error) {  
                    infoDiv.innerHTML = '<span style="color:#c62828;">กรุณาอนุญาตการเข้าถึงตำแหน่ง (GPS) ในเบราว์เซอร์ของคุณ</span>';  
                }, { enableHighAccuracy: true });  
            } else {  
                infoDiv.innerHTML = 'เบราว์เซอร์ของคุณไม่รองรับการระบุตำแหน่ง';  
            }  
        } else if (now < startTime) {  
            statusDiv.innerHTML = '<span class="expired">สถานะ: ยังไม่ถึงเวลาเปิดใช้งาน</span>';  
            infoDiv.innerHTML = '<p>ลิงก์นี้จะเปิดให้ใช้งานตามเวลาที่กำหนดไว้เท่านั้นครับ</p>';  
        } else {  
            statusDiv.innerHTML = '<span class="expired">สถานะ: ลิงก์หมดอายุแล้ว</span>';  
            infoDiv.innerHTML = '<p>พ้นช่วงเวลาที่กำหนดในการแชร์ตำแหน่งแล้วครับ</p>';  
        }  
    </script>  
</body>  
</html>  
  
