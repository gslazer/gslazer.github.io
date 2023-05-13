---
title: "Mac Visual Studio Code를 사용한 Unity개발환경 세팅 로그 및 트러블 슈팅"
date: 2023-05-14 03:31:28 -0000
categories: Mac Unity VisualStudioCode C#
---

# Mac Visual Studio Code를 사용한 Unity개발환경 세팅 로그 및 트러블 슈팅

# VS Code 개발환경 구축

Mac OS / Visual Studio Code 기준으로 

Unity3d와 VS Code를 연동한 작업환경을 목적으로 Research를 진행.

개발환경 기록및 문서화를 위한 자료 수집을 목적으로 본 문서에 기록하며 진행한다.

# Refference Source

[Set up Visual Studio Code with UNITY, MAC, and INTELLISENSE WORKING 2023](https://www.youtube.com/watch?v=3GVGyooZ8jk)

2023년 초에 작성된 아래 가이드 영상으로, 세팅 try과정의 대부분을 참고하였다.

[](https://code.visualstudio.com/docs/other/unity)

vs code 공식 사이트에서의 unity 연동 관련 문서.

그 외 Trouble Shooting 과정에서 확인하거나 도움된 페이지는 아래에 추가로 기록한다.

# 차례

위 영상에서 추천하는 구축 순서를 대부분 따라 해 본다. 

물론 23년 5월 현재 이걸 그대로 따라한다고 해도 vscode 와 unity editor가 완전하게 연동되지 않는것을 먼저 확인했기 때문에, 스탭별로 정보를 모아 해결해보도록 하겠다.

> 1. Install Visual Studio Code for Mac and go through the installation process
2. Close out of Unity and Visual Studio Code
3. Install Mono Stable Channel for and .NET SDK for Visual Studio Code
4. I recommend restarting after installing these
5. Open Unity, install the VS Code package if not installed, set External Tools to Visual Studio Code, Click Regenerate Project Files
6. Edit -- Open C# Assets (if you ever have a problem with VS Code not recognizing your code, close out of VS Code and Open it with this button)
7. Download C#, Unity Tools, Unity Debugger, Unity Code Snippets, Unity Snippets,  C# XML Documentation, using Halcyon Theme
8. Settings: -Omnisharp: Use Modern Net Turn Off. Set Omnisharp Mono Path (/Library/Frameworks/Mono.framework/Versions/Current”, Disable Telemetry (collect analytics on you), Turn off CodeLens, inlay hints parameters c#,
9. Close VS Code, word wrap to word wrap column (search wrap), wrapping indent (indent)
10. Edit -- Open C# Assets 
11. Make sure Intellisense is working, fire in bottom right, opens up terminal and tells you if there are any errors, choose correct solution
12. Go to Debug Tab, Create Launch.json Unity Debugger, Place Breakpoint, Press Play, Go to Unity and Enable Debugging Session, Press Play in Unity, then you can step through the code and see details
> 

# 실행 스탭

1. Visual Studio Code, Unity hub, Unity Editor 설치
2. Unity와 VSC를 닫을 것
3.  .net SDK(arm-64)와 Mono framework(Stable Channel)를 설치
    1. dot net SDK ( [https://dotnet.microsoft.com/en-us/download](https://dotnet.microsoft.com/en-us/download) )
    arm-64 버전으로
    2. mono framework([https://www.mono-project.com/download/stable/](https://www.mono-project.com/download/stable/) )
    해당 영상에서는 Stable Channel을 추천하지만, 
    ([https://www.androidhuman.com/2020-09-14-unity_with_vscode](https://www.androidhuman.com/2020-09-14-unity_with_vscode))에서는 Visual Studio Channel을 추천한다.)
4. 재시동 할것.
5. Unity를 열고, Package Manager에서 VS Code Package를 설치, External Tools에서 VisualStudioCode를 설정 후, Regenerate Project Files
    
    ![Untitled](Mac%20Visual%20Studio%20Code%E1%84%85%E1%85%B3%E1%86%AF%20%E1%84%89%E1%85%A1%E1%84%8B%E1%85%AD%E1%86%BC%E1%84%92%E1%85%A1%E1%86%AB%20Unity%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%E1%84%92%E1%85%AA%E1%86%AB%E1%84%80%E1%85%A7%20bfee9ed136ba408da254af9fd7263daa/Untitled.png)
    
6. Open C# Assets - (유니티에서 VS Code 실행) 
7. VS Code 에서 아래 플러그인들 설치
Download C#, Unity Tools, Unity Debugger, Unity Code Snippets, Unity Snippets,  C# XML Documentation, using Halcyon Theme
*(나는 덕후테마가 많이 포함된 Doki Theme를 추가로 설치했다.)*
    
    
8. **(중요) Settings - Omnisharp : Use Modern Net 체크 해제.**
    
    ![Untitled](Mac%20Visual%20Studio%20Code%E1%84%85%E1%85%B3%E1%86%AF%20%E1%84%89%E1%85%A1%E1%84%8B%E1%85%AD%E1%86%BC%E1%84%92%E1%85%A1%E1%86%AB%20Unity%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%E1%84%92%E1%85%AA%E1%86%AB%E1%84%80%E1%85%A7%20bfee9ed136ba408da254af9fd7263daa/Untitled%201.png)
    
    이 시점에서 이런 에러를 맞이하게된다.
    
9. Mono Path 설정
*CodeLens, inlay hints parameters c#를 disable할것을 추천 (나는 나름 유용하게 쓰는 기능이라 남기고 싶다.???)*
10. *VS Code를 닫고 word wrap to word wrap column(스트링 관련 플러그인 설정…딱히 필요 없어보인다.)*
11. Edit — Open C# Assets
    
    
    ![문제 1- OmniSharp requirese a complete install of Mono ](Mac%20Visual%20Studio%20Code%E1%84%85%E1%85%B3%E1%86%AF%20%E1%84%89%E1%85%A1%E1%84%8B%E1%85%AD%E1%86%BC%E1%84%92%E1%85%A1%E1%86%AB%20Unity%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%E1%84%92%E1%85%AA%E1%86%AB%E1%84%80%E1%85%A7%20bfee9ed136ba408da254af9fd7263daa/Untitled%202.png)
    
    문제 1- OmniSharp requirese a complete install of Mono 
    
    역시 다시 이 오류에 부딪힌다…..
    
    - https://github.com/OmniSharp/omnisharp-vscode/issues/5445 의 정보를 토대로 1.25.0으로 C# extention(Omnisharp)를 다운그레이드
    이슈에서는 1.25.1에서의 같은 문제가 1.25.0으로 다운그레이드로 해결됨이 제시되었다.
    - 맥에서의 1.25.7(현재 버전)에서의 같은 문제 역시 보고되어 있다.
    - 영상에서 정상동작에 사용된 확장 버전이 1.25.0에서 확인 후 1.25.2 및 1.25.4까지 확인해보도록 한다.(1.25.5, 1.25.6 버전은 존재하지 않는다.)
    - 1.25.0버전, 1.25.4 확인, .NET Core SDK에 대한 문제가 추가로 발생했다.
        
        ![문제 2 - The .NET Core SDK cannot be located.](Mac%20Visual%20Studio%20Code%E1%84%85%E1%85%B3%E1%86%AF%20%E1%84%89%E1%85%A1%E1%84%8B%E1%85%AD%E1%86%BC%E1%84%92%E1%85%A1%E1%86%AB%20Unity%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%E1%84%92%E1%85%AA%E1%86%AB%E1%84%80%E1%85%A7%20bfee9ed136ba408da254af9fd7263daa/Untitled%203.png)
        
        문제 2 - The .NET Core SDK cannot be located.
        
        이번에는 .NET core를 패스에서 찾지 못하는 문제 확인. 
        
    - 여기에서 Get the SDK의 링크가 우리가 받은 dot net SDK 의 링크와 다르다.
        
        [Visual Studio Code용 .NET SDK 다운로드](https://dotnet.microsoft.com/ko-kr/download/dotnet/sdk-for-vs-code?utm_source=vs-code&amp;utm_medium=referral&amp;utm_campaign=sdk-install)
        
        링크에 존재하는 Visual Studio Code 용 Dot net SDK를 다운받는다.
        
    - Visual Studio Code용 Dot net SDK를 설치한 후 문제 2 - The .NET Core SDK cannot be located. 는 해결되었지만 문제 1- OmniSharp requirese a complete install of Mono 는 여전했다.
    - Mono를 Visual Studio Channel 로 다시 다운로드하여 설치한 결과, 문제 1- OmniSharp requirese a complete install of Mono 이 사라졌다.
        
        ![문제 1과 문제 2가 모두 해결된 상태](Mac%20Visual%20Studio%20Code%E1%84%85%E1%85%B3%E1%86%AF%20%E1%84%89%E1%85%A1%E1%84%8B%E1%85%AD%E1%86%BC%E1%84%92%E1%85%A1%E1%86%AB%20Unity%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%E1%84%92%E1%85%AA%E1%86%AB%E1%84%80%E1%85%A7%20bfee9ed136ba408da254af9fd7263daa/Untitled%204.png)
        
        문제 1과 문제 2가 모두 해결된 상태
        
        ![OmniSharp가 정상동작하는 상태](Mac%20Visual%20Studio%20Code%E1%84%85%E1%85%B3%E1%86%AF%20%E1%84%89%E1%85%A1%E1%84%8B%E1%85%AD%E1%86%BC%E1%84%92%E1%85%A1%E1%86%AB%20Unity%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%E1%84%92%E1%85%AA%E1%86%AB%E1%84%80%E1%85%A7%20bfee9ed136ba408da254af9fd7263daa/Untitled%205.png)
        
        OmniSharp가 정상동작하는 상태
        
    
    이로써 참조가 동작하는 상태에 진입했다. 드디어 Intellisense와 Debug가 동작하는지 확인하는 단계에 진입할 수 있게 되었다.
    
12. Intellisense가 동작하는지 확인, 터미널에 오류가 뜨는지 확인, 올바른 솔루션이 열렸는지 확인.
    
    vs code의 Debugger메뉴에서 Unity Debugger를 통해 Launch.json을 생성한다.
    vs code에서 Dubug를 시작하면 Editor에서 뜬 창에서 아래 창이 팝업되고, debugging for session을 Enable해주면 연동이 완료되어 디버그 가능한 상태가 된다.
    
    이제 Unity에서 실행버튼을 눌러 디버그를 확인하면 끝.
    
    ![VS Code에서의 연결을 알리는 팝업](Mac%20Visual%20Studio%20Code%E1%84%85%E1%85%B3%E1%86%AF%20%E1%84%89%E1%85%A1%E1%84%8B%E1%85%AD%E1%86%BC%E1%84%92%E1%85%A1%E1%86%AB%20Unity%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%E1%84%92%E1%85%AA%E1%86%AB%E1%84%80%E1%85%A7%20bfee9ed136ba408da254af9fd7263daa/Untitled%206.png)
    
    VS Code에서의 연결을 알리는 팝업
    
    ![BreakPoint로 디버그 상태를 테스트하여 마무리](Mac%20Visual%20Studio%20Code%E1%84%85%E1%85%B3%E1%86%AF%20%E1%84%89%E1%85%A1%E1%84%8B%E1%85%AD%E1%86%BC%E1%84%92%E1%85%A1%E1%86%AB%20Unity%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%E1%84%92%E1%85%AA%E1%86%AB%E1%84%80%E1%85%A7%20bfee9ed136ba408da254af9fd7263daa/Untitled%207.png)
    
    BreakPoint로 디버그 상태를 테스트하여 마무리
    

# 중요 문제해결 포인트

- Dot Net SDK는 VS Code용 다운로드 페이지에서 받은것을 사용할 것

[Visual Studio Code용 .NET SDK 다운로드](https://dotnet.microsoft.com/ko-kr/download/dotnet/sdk-for-vs-code?utm_source=vs-code&amp;utm_medium=referral&amp;utm_campaign=sdk-install)

- Mono는 Visual Studio Channel을 사용할 것

[Download - Stable | Mono](https://www.mono-project.com/download/stable/#download-mac)

- 그럼에도 [문제 1- OmniSharp requirese a complete install of Mono]가 발생할 경우, C# 확장 버전을 1.25.0 또는 1.25.4로 다운할 것

# 보완점, 리뷰

사실 불필요한 과정이 포함되어 있을 것 같은데, 그래도 VS Code 테스트환경을 따라해보는것은 도움이 되었다.

(해놓고 보니 쓸데없는 몇가지 과정을 진행하기 전에 디버그가 잘 되는걸 본 것 같은 기분이.. -_-;;;;)

한번더 환경을 초기화 하고 세팅 순서를 단순화할 여지가 있어 보인다. (언제가 될지 모르겠지만)다음 문서에서 결론만 추려서 남길 가이드 문서 작업을 미래의 내게 맡기며 이 문서를 닫도록 하자 ^^!

# 참고 자료

[Set up Visual Studio Code with UNITY, MAC, and INTELLISENSE WORKING 2023](https://www.youtube.com/watch?v=3GVGyooZ8jk)

[[Unity] 유니티 입문 하기_C# 프로그래밍 기본_첫 게임 제작하기](https://myeonguni.tistory.com/1802)

[VS Code로 유니티 개발하기 (macOS 사용자를 위한 팁 포함)](https://www.androidhuman.com/2020-09-14-unity_with_vscode)