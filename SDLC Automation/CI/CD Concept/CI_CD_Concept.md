<h1> CI/CD Concept </h1>

<h2> Vấn đề </h2>

  - Mô hình phát triển phần mềm: waterfall (thác nước):
    + Slow.
    + Not iterative.
    + Long release cycles.

    => Thực tế chúng ta muốn điều ngược lại: fast, iterative, short release cycles
    
    => Vấn đề chúng ta cần giải quyết.

<h2>DevOps</h2>

  - Dev và Ops không có chung mục tiêu.
  - Dev:
    + Có bao nhiêu thay đổi ?
    + Tính năng và lỗi có thể đẩy lên production ?
    + Làm cách nào để thực hiện chúng nhanh nhất có thể ?
  - Operations:
    + Muốn hệ thống ổn định và khả dụng.

  => Thực tế, họ muốn tất cả: phát hành nhanh, hệ thống ổn định.

  => Devops: Hợp nhất Dev, Ops và QA.

  - Mục tiêu: Rút ngắn vòng đời phát triển, cung cấp các tính năng, bản sửa lỗi và cập nhật thường xuyên.

  ![Devops Model](/SDLC%20Automation/pattern_devops.webp)

<h2> CI/CD</h2>

  - CI: Continuous integration - tích hợp liên tục.
  - CD: Continuous delivery - giao hàng liên tục.
  - Continuous Delivery hay Continuous Development:
  
  ![Continuous Delivery & Continuous Development](/SDLC%20Automation/CI_CD.png)

<h2> Developer Tools on AWS </h2>

  - CodeCommit: Repository ( ~ Github).
  - CodeBuild: Build our code ( ~ Java => Result: JAR file).
  - CodeDeploy: Deploy our build.
  - CodePipeline: Quản lý và tự động hoá tất cả những tác vụ trên.

<h2> Exam Tips </h2>

  - Continuous Deployment: Là toàn bộ quá trình triển khai lên môi trường production mà không có các biện pháp bảo vệ giao hàng liên tục (continuous delivery).
  - Continuous Delivery: Có dấu hiệu dừng, chúng ta muốn tự động nhiều nhất có thể, nhưng chúng ta sẽ kiểm tra an toàn lần cuối trước khi triển khai lên môi trường production 