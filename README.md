<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Điểm danh lớp học</title>
</head>
<body>
    <h1>Điểm danh lớp học</h1>
    <p id="status">Đang kiểm tra vị trí của bạn...</p>
    <div id="formContainer" style="display: none;">
        <!-- Nhúng Google Form -->
        <iframe 
            id="googleForm" 
            src="https://docs.google.com/forms/d/e/1FAIpQLSeOS07AKAvjxizsMvlMAesAD2fRIpftOch_P3SKcSZLxNqwIA/viewform?embedded=true" 
            width="100%" 
            height="800" 
            frameborder="0">
        </iframe>
    </div>

    <script>
        // Tọa độ lớp học
        const CLASS_LAT = 10.735312; // Vĩ độ lớp học
        const CLASS_LNG = 106.671735; // Kinh độ lớp học
        const RADIUS = 100; // Bán kính cho phép (mét)

        // Hàm tính khoảng cách giữa hai điểm
        function distance(lat1, lon1, lat2, lon2) {
            const R = 6371e3; // Bán kính Trái đất (mét)
            const φ1 = lat1 * Math.PI / 180;
            const φ2 = lat2 * Math.PI / 180;
            const Δφ = (lat2 - lat1) * Math.PI / 180;
            const Δλ = (lon2 - lon1) * Math.PI / 180;

            const a = Math.sin(Δφ / 2) * Math.sin(Δφ / 2) +
                      Math.cos(φ1) * Math.cos(φ2) *
                      Math.sin(Δλ / 2) * Math.sin(Δλ / 2);
            const c = 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1 - a));
            return R * c; // Khoảng cách (mét)
        }

        // Kiểm tra vị trí
        if (navigator.geolocation) {
            navigator.geolocation.getCurrentPosition(
                position => {
                    const userLat = position.coords.latitude;
                    const userLng = position.coords.longitude;
                    const dist = distance(CLASS_LAT, CLASS_LNG, userLat, userLng);

                    if (dist <= RADIUS) {
                        document.getElementById('status').textContent = "Bạn đang ở trong lớp học. Hãy điền biểu mẫu.";
                        document.getElementById('formContainer').style.display = "block";
                    } else {
                        document.getElementById('status').textContent = "Bạn không ở trong lớp học. Không thể điểm danh!";
                    }
                },
                error => {
                    document.getElementById('status').textContent = "Không thể xác định vị trí. Hãy bật GPS và thử lại.";
                }
            );
        } else {
            document.getElementById('status').textContent = "Trình duyệt của bạn không hỗ trợ định vị.";
        }
    </script>
</body>
</html>
