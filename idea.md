1. Tổng quan project: Quản lý điểm của học sinh
2. Phase 1: Quản lý điểm của học sinh
3. Màn hình login có thể login cho nhiều role
4. Role admin system: tài khoản là adminstpoint@gmail.com / adminstpoiNt1122@
5. Role admin system có thể CRUD danh sách trường học, không cần quản lý gì thêm
6. Khi admin system tạo trường học, tạo luôn account admin school theo trường học đó luôn
7. Role admin school là tài khoản quản lý thông tin học sinh, giáo viên, lớp học
8. Role admin school có thể tạo danh sách các lớp học.
9. Role admin school sẽ tạo master data như sau
9.1. Năm học: khi tạo năm học chỉ cần các thông tin sau: tên năm học, thời điểm bắt đầu, thời điểm kết thúc, trạng thái (đang diễn ra / đã kết thúc / chuẩn bị diễn ra), trong 1 thời điểm chỉ có 1 năm học đang diễn ra
9.2. Thông tin giáo viên: bao gồm các thông tin của giáo viên như sau: Họ và tên, ngày tháng năm sinh, căn cước công dân với căn cước công dân là unique, trạng thái, mã số (unique, key), giới tính, trạng thái (đang làm việc / đã nghỉ), email, password (để đăng nhập role giáo viên)
9.3. khi nhấn vào chi tiết năm học sẽ hiển thị danh sách môn học: tên môn học, năm học.
9.4. Thông tin học sinh: Danh sách học sinh bao gồm các thông tin sau: Họ tên, ngày tháng năm sinh, địa chỉ, giới tính, email, mật khẩu đăng nhập, thông tin phụ huynh (bao gồm: Họ tên cha, nghề nghiệp của cha, ngày sinh của cha, họ tên mẹ, nghề nghiệp mẹ, email, số điện thoại, mật khẩu (email và mật khẩu dùng để đăng nhập cho role phụ huynh)), trạng thái (đang học / đã nghỉ), ID
9.4. Thông tin lớp học: danh sách lớp học bao gồm các thông tin cơ bản như sau: tên lớp, năm học, giáo viên chủ nhiệm, trong 1 năm học không được có lớp trùng tên, và chỉ 1 giáo viên chỉ được chủ nhiệm 1 lớp. Nếu trùng tên hệ thống hiển thị msg báo lỗi và tạo lớp đó không thành công. Đối với chọn giáo viên chủ nhiệm, khi trong năm đó, giáo viên A đã chủ nhiệm lớp 1A rồi thì khi tạo lớp 1B và 1C lúc chọn giáo viên chủ nhiệm không còn hiển thị tên giáo viên A nữa
10. Khi role admin school vào chi tiết lớp học, sẽ có mục danh sách học sinh. Khi nhấn vào btn "chỉnh sửa danh sách học sinh" sẽ hiển thị pop-up danh sách học sinh có trạng thái là đang học, admin school sẽ click vào check box sau đó click thêm vào lớp để thêm học sinh vào danh sách. Trong cùng 1 năm học, 1 học sinh chỉ được ở 1 lớp học
11. Khi Role admin school vào chi tiết lớp học, có btn thông tin giáo viên bộ môn, khi click vào đó sẽ hiển thị page mới thông tin bộ môn. Hiển thị dưới dạng danh sách gồm 2 cột: tên môn học và giáo viên phụ trách. Tại tên môn học, khi click vào droplist môn học sẽ hiển thị ra danh sách môn học của năm học đó, khi click vào drop list giáo viên, sẽ hiển thị danh sách giáo viên để có thể chọn. Khi cần thêm 1 dòng dữ liệu mới, click button thêm mới
12. Khi giáo viên đăng nhập vào hệ thống, màn hình dashboad hiển thị lên với các button sau: Lớp chủ nhiệm (nếu giáo viên đó được chọn làm giáo viên chủ nhiệm của lớp đó trong năm đó) và lớp bộ môn (giáo viên được add làm giáo viên bộ môn của lớp nào sẽ hiển thị danh sách lớp mà giáo viên đó phải phụ trách bộ môn trong năm đó)
13. Khi giáo viên click vào lớp chủ nhiệm, hiển thị bảng điểm trung bình theo từng môn của học sinh lớp đó
13.1. Khi giáo viên click vào button bảng điểm của học sinh sẽ hiển thị bảng điểm chi tiết của học sinh đó. 
13.2. Khi giáo viên click vào button thông tin của học sinh sẽ hiển thị thông tin cá nhân của học sinh đó.
13.4. khi giáo viên click vào button nhận xét của học sinh sẽ hiển thị danh sách nhận xét của học sinh đó.
13.4.1. Lịch sử nhận xét được sắp xếp theo thứ tự mới nhất, thời điểm gửi, và trạng thái
13.4.2. Khi nhấn tạo nhận xét, sẽ phải nhập các thông tin như sau: thời gian gửi (là tương lai), nội dung nhận xét
13.4.3. Khi mới tạo xong, trạng thái của nhận xét là `Đã lên lịch ` và có thể edit lại (có thể edit lại được cả lịch gửi)
13.4.4. khi đến thời điểm seting, phía account phụ huynh và học sinh được nhận xét đó sẽ nhận được 1 thông báo với nội dung ` nhận xét mới từ giáo viên + tên giáo viên `. và sau đó, trạng thái phía giáo viên đổi thành ` Đã gửi `
14. Khi giáo viên click vào button lớp bộ môn tại màn hình dashboard, hiển thị danh sách lớp mà giáo viên đó phụ trách bộ môn với các thông tin sau: lớp và bộ môn mà giáo viên đó phụ trách
14.1. Khi click vào xem chi tiết, sẽ hiển thị bảng điểm chi tiết của học sinh trong bộ môn đó, có thể chỉnh sửa trực tiếp tại bảng sau đó nhấn lưu, điểm của học sinh lúc đó sẽ được tính toán lại
14.2. Khi cần thêm 1 cột điểm mới cho học sinh, click vào button thêm điểm, tại title cột sẽ chọn được hệ số để nhập điểm vào.
15. Cách tính điểm trung bình của học sinh: giả sử học sinh đó có 3 con điểm hệ số 1 lần lượt là (7,8,9) 2 con điểm hệ số 2 lần lượt là 7 và 9 và 1 con điểm hệ số 3 là 8 thì cách tính trung bình là (7+8+9+2*(7+9)+3*8)/ (1+1+1+2+2+3) trong đó (1+1+1+2+2+3) là tổng hệ số
16. Phụ huynh sau khi đăng nhập chỉ có thể xem bảng điểm và nhận xét về con của mình
17. Học sinh sau khi đăng nhập chỉ có thể xem bảng điểm và nhận xét về chính mình