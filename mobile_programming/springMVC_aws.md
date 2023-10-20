# MVC
M  V  C
> * M : model DB
> * V : View (HTML, CSS, javascript)
> * C : Controller (사용자 요청을 처리)
***

# AWS Controller

### 업로드 후, 파일을 가져오는 코드

```java
@RestController //api 방식이기 때문에 restcontroller을 사용함
@RequiredArgsConstructor
@RequestMapping("/api/v1/rest/aws") //controller를 사용할 수 있는 주소이다.
public class AWSController {
    private final AWSService awsController;

    @GetMapping("/list") //파일 리스트를 가져오는 부분
    private List<String> onFileList() {
        return awsController.getFileList();
    }

    @PostMapping("/upload") //upload 부분
    private ModelAndView onUpload(@RequestParam MultipartFile file) { //
        try {
            awsController.upload(file);
            return new ModelAndView("redirect:/?success=true"); //성공으로 파일이 업로드 됐다면 return true
        }catch (Exception ex) {
            ex.printStackTrace();
            return new ModelAndView("redirect:/server-error?errorStatus=" + ex.getMessage()); //에러 메시지 출력
        }
    }
}
```

### data.html
'''html
<html lang="ko">
    <body>
        <h2> AWS API에 연결되었습니다! </h2>
        AWS API에 성공적으로 연결되었습니다. 현재 파악된 파일 목록은 다음과 같습니다 : <br/>
        <ul id="fileList">

        </ul>
        <form  method="post" action="api/v1/rest/aws/upload" enctype="multipart/form-data">
            <input name = "file" type="file" value="파일 선택"/><br/>
            <input type="submit" value="업로드"/><br/>
        </form>
        <span id="message"> </span>
    </body>
    <script>
        (async () => {
            const response = await fetch('/api/v1/rest/aws/list');
            const result = await response.json();
            console.log(result);
            for (let i = 0; i < result.length; i++) {
                let li = document.createElement("li");
                li.appendChild(document.createTextNode(result[i]));
                document.getElementById("fileList").appendChild(li);
            }
        })()

        let params = new URLSearchParams(window.location.search);
        if (params.get("success") != null) {
            document.getElementById("message").innerHTML = "파일 업로드에 성공하였습니다.";
        }
    </script>
</html>
'''