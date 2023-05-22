<h2> CodeDeploy </h2>

<h3> Định nghĩa </h3>

- Là dịch vụ triển khai tự động hóa việc triển khai ứng dụng đến AWS EC2 instances, on-premises instances, Lambda và ECS.

- Tips
    - Kho lưu trữ phổ biến nhất cho CodeDeploy Artifacts là S3.
    - EC2/on-premises deployments có thể sử dụng cả GitHub và BitBucket.

<h3> Process Config CodeDeploy </h3>

- Step 1: Cung cấp IAM cho người dùng hoặc 1 nhóm người dùng có quyền truy cập vào CodeDeploy (đính kèm CodeDeploy policy cho người dùng).
- Step 2: Tạo IAM instance profile cho phép EC2 access đến kho lưu trữ.
- Step 3: Tạo IAM service role để cấp quyền truy cập cho CodeDeploy vào các tài nguyên AWS khác (tạo từng service role cho AWS EC2, Lambda, và EC2 nếu cần)

- Adding automation:

    ![Automating Notifications](/SDLC%20Automation/CodeDeploy/add_setting_noti_codedeploy.PNG)

<h3> CodeDeploy App, Deployment Groups, and Deployment Configurations</h3>

- What is an application? 
    - Trong CodeDeploy, nó chỉ là 1 định danh được sử dụng để tham chiếu tới cài đặt triển khai của chúng ta.
- What is a deployment group ?
    - Nó là instance hoặc instances mà chúng ta muốn đó là mục tiêu và triển khai code của chúng ta. 
- What is a deployment configurations ?
    - Là 1 bộ quy tắc và điều kiện thành công/thất bại được sử dụng bởi CodeDeploy trong quá trình triển khai. Vì vậy, các quy tắc là khác nhau tùy thuộc vào nơi chúng ta triển khai (EC2 instance, on-premises instance, Lambda hoặc ECS)
        + EC2/On-premises: Là số lượng instances mà chúng ta muốn triển khai mã của mình.

<h3> Deployment Step </h3>

1. Tạo application
2. Tạo Deployment Group
3. Chỉ định cấu hình triển khai (Deployment Config)
4. Upload revision (tải lên bản sửa đổi của mình) - Package mà chúng ta muốn triển khai - JarFile
5. Deploy
6. Kiểm tra kết quả.
7. Redeploy nếu cần.

<h3> Default Deployment Options </h3>

- Chúng ta có thể thực hiện:
    - Từng cái một (one at a time)
        - Ngăn ngừa rủi ro và nó an toàn nếu có sự cố xảy ra.
    - Tất cả cùng một lúc (all at once)
        - Có thể sẽ có thời gian chết (downtime) và chúng ta cũng có thể có 1 pha rollback lộn xộn.
    - MỘt nửa một (Half at a time)
        + Cung cấp một số biện pháp bảo vệ trong TH xảy ra sự cố, và mặt trái của nó là tốc độ triển khai.

<h3> CodeDeploy appspec.yml</h3>

- Tương tự như CodeBuild có buildspec, thì appspec file dành cho CodeDeploy, format của nó có thể là yaml hoặc json, giống với CloudFormation templates.
- Được sử dụng để quản lý từng triển khai dưới dạng series của vòng đời sự kiện (hooks) (Auto Scaling có các hook trong vòng đời sự kiện).
- Appspec overview:

    ![AppSpec Overview](/SDLC%20Automation/CodeDeploy/appspec_overview.png)

- Định nghĩa:
    + Nó quản lý việc triển khải của chúng ta.
    + Duy nhất đồi với CodeDeploy.
    + Format: YAML hoặc JSON.
- Cấu trúc file:
    + Được sử dụng để quản lý mỗi lần triển khai dưới dạng một loạt móc nối sự kiện vòng đời, được xác định trong file.
    + Các options:
        + version number: required.
        + OS: required, Linux, Windows.
        + files: chỉ định các tệp được sao chép cho instance trong quá trình triển khai: optional.
        + hooks: BeforeInstall, AfterInstall, ApplicationStart, ValidateService (last)....
            + Trong các sự kiện, chúng ta có thể chạy các tập lệnh.
            
<h3> CodeDeploy Revision và Deployment </h3>

<h4> CodeDeploy Revision</h4>

- Application Revisions: 

+ Bản sửa đổi chỉ đơn giản là gói (ZIP/TAR/JAR file) chứa các nhóm tệp hiện tại mà chúng ta muốn deploy.
+ Là 1 bản cập nhật/sửa lỗi của ứng dụng.
+ CodeDeploy: 
    - Chúng ta cần 1 loại kho lưu trữ, nơi chúng ta sẽ lưu trữ mã nguồn đi kèm: S3,...
    - Gói các tệp nguồn (Bundle the source) và đẩy chúng vào kho lưu trữ đã chọn ở trên.

<h4> Deployment </h4>

- Chúng ta deploy bản sửa đổi này bằng cách nào ?
    + Code của chúng ta lưu trữ ở S3.
    + CodeDeploy có thể lấy bản sửa đổi đó.
    + CodeDeploy triển khai bản sửa đổi này lên Deployment Group (lựa chọn cách triển khai phù hợp).
