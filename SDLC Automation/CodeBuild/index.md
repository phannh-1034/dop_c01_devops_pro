<h2> CodeBuild</h2>

<h3> Định nghĩa </h3>

- Là dịch vụ tích hợp liên tục được quản lý hoàn toàn để biên dịch mã nguồn, chạy thử nghiệm và tạo các gói phần mềm sẵn sàng triển khai.
- Chúng ta ko cần lo lắng về quá trình mã hóa dữ liệu trong quá trình vận chuyển và at rest.
- Không cần phải duy trì các máy chủ do AWS đã đảm nhiệm.
- Có thể được thêm vào trong giai đoạn build và giai đoạn thử nghiệm trong pipeline.

<h3> Output </h3>

- CodeBuild sản xuất các gói phần mềm (software package) đã sẵn sàng để triển khai: giống như .jar file.

<h3> Đặc điểm </h3>

- Chúng ta không cần phải lo lắng về máy chủ và tính sẵn sàng, do AWS đã đảm nhiệm.
- Khả năng mở rộng: có thể thay đổi để phù hợp với quy mô, những gì chúng ta bị tính phí là khi các bản build của chúng ta đang chạy. Vì vậy nó không phải 1 khoản phí dựa trên phần cứng mà là dựa trên số lượng hoạt động chúng ta đang thực hiện bản build đang chạy trong bao lâu. Có thể mở rộng/thu nhỏ theo nhu cầu sử dụng của chúng ta.

<h3> Access CodeBuild </h3>

- Connect CodeBuild by:
    
    + Management Console.
    + CLI.
    + SDK.
    + CodePipeline.

- Khi làm việc với CodeBuild, tức là chúng ta đang làm việc với code, và chúng ta sẽ có những hiện vật (artifacts) từ các bản build của chúng ta
    => Làm cách nào để giữ các hiện vật(artifacts) đó ?
    => Chúng ta sẽ giữ chúng trên S3.

- Vì vậy, ban đầu, chúng ta lấy mã từ kho lưu trữ (CodeCommit,...) =>  CodeBuild (tạo ra các artifacts) => S3 (versioning và encyryption).

<h3> Build Project </h3>

- Về cơ bản, build project sẽ nói với CodeBuild cách chạy bản build, bao gồm cả nơi lấy mã nguồn, môi trường xây dựng nào sẽ được sử dụng, lệnh xây dựng nào sẽ được chạy, và nơi lưu trữ đầu tra của bản build đó.

<h3> CodeBuild Walkthrough</h3>

![CodeBuild Diagram](/SDLC%20Automation/CodeBuild/CodeBuild.png)

Một số bài toán cụ thể:

1. Chúng tôi muốn chạy CodeBuild saui khi merge code thành công trên CodeCommit ?
    
    Answer: Setup CloudWatch để theo dõi việc merge code lên CodeCommit và trigger build trên CodeBuild.

2. Chúng tôi muốn gửi nhật ký CodeBuilds và gửi notify khi nó new logs.

    Answer:
        
    - Send CodeBuild Logs to CloudWatch Logs.
    - Setup CloudWatch event (Event Bridge) để theo dõi cho logs mới.
    - Trigger Lambda function để gửi notification to Slack.

<h3> Review Time </h3>

- Input: Chúng ta cần cung cấp CodeBuild với build project.
- Output: Nếu có bất kỳ đầu ra bản dựng nào, thì môi trường bản dựng sẽ tải đầu ra của nó lên bộ chứa S3.
- Chỉ khi các tạo phẫm mã (code artifacts) của chúng ta cần được xây dựng/đóng gói, thì mới cần build stage.
- buildspec: tập hợp các lệnh build và settings liên quan ở địngh dạng YAML, mà CodeBuild sử dụng để chạy build.

<h3> Building Plan </h3>

- Source lưu ở đâu ?
    - Có thể ở S3 ? Có thể là CodeCommit.
- Các lệnh build nào và thứ tự ra sao: xây dựng trong buildspec (format yaml và được đặt ở root folder).
- Runtimes và tools cần sử dụng là gì: Java, Maven..
- Service có thể hoạt động với các dịch vụ AWS khác mà chúng ta cần ? (CloudFormation, Codebuild, Lambda)
- CodeBuild có hoạt động với VPC của bạn không ? 
    - Điều này sẽ xảy ra nếu chúng ta đang build và làm việc với EC2 theo định nghĩa, EC2 sẽ có trong VPC.

    => Điều chúng ta cần là gì ?
    - VPC ID.
    - Security groups.
    - Subnet IDs.