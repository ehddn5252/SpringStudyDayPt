# pull request 과정

먼저 계정 2개가 필요하다. 그래서 계정 하나를 더 만들고 진행했다.

### 여기에서 하는 일은 다음과 같다.
- 부 계정(부계정 name은 woodongIsGood이다.)에 repository를 만들었다.
- 본 계정(본계정 name은 ehddn5252이다.)은 부 계정의 repository에 pull request를 한다.
- 부 계정에서 본 계정에서 온 pull request를 확인하고 merge 한다. (변경할 사항을 받아준다.)
- 본 계정에서 부 계정에서 수정된 것을 pull 받는다.


- 추가로 pull request를 한번 더 보낸다.

1. 대상 레포지토리를 fork 한다.
    ![image](https://user-images.githubusercontent.com/51036842/164957657-2b98de1e-b773-49f7-859e-5acb4a3cd948.png)


2. clone 받는다.<br>
    가져온 repository에서 복사 버튼을 누르면 된다.
    ![image](https://user-images.githubusercontent.com/51036842/164957705-903fb476-3b27-4ffc-969d-dbacc8965a31.png)

    그 다음 로컬 스토리지에 clone 하면 된다.

    ![image](https://user-images.githubusercontent.com/51036842/164957715-2b1d6f4a-61ac-477a-83f8-9d1560072bc5.png)

    ```
    git clone https://github.com/ehddn5252/SpringStudy.git
    ```

3. remote 설정을 한다. <br>
    remote 설정은 내가 원격 저장소에 pull 요청할 곳을 추가한다고 생각하면 된다. 그래서 pull 요청할 부계정 원격 저장소의 주소를 해당 별명으로 저장해놓는다.

    여기서 나는 부계정 원격저장소의 별명을 prSS 로 설정했다.

    <span style="color:yellow">원래 명령어</span>

    ```
    git remote add "별명" https://github.com/원본계정/blog.github.io.git
    ```
    <span style="color:green">내 명령어</span>
    ```
    git remote add prSS https://github.com/woodongIsGood/SpringStudy.git
    ```

    ![image](https://user-images.githubusercontent.com/51036842/164957924-293f2608-b500-4319-9b04-a0267c90e54a.png)



4. 작업할 branch를 생성하고 작업할 branch로 옮긴다..

    <span style="color:yellow">원래 명령어</span>

    ```
    git checkout -b <branch 명>
    ```

    <span style="color:green">내 명령어</span>

    ```
    git checkout -b develop
    ```

    ![image](https://user-images.githubusercontent.com/51036842/164957986-d6f57547-b0ce-4b60-b2db-7edcc8f77cbc.png)


    그러면 현재 develop branch에 있다. 

5. 작업한다.
    - 내가 pr 할 내용을 작업한다.
    <BR> 폴더 하나 만들어서 MD 파일 있던 것을 넣었다.

    ![image](https://user-images.githubusercontent.com/51036842/164958074-21ff20e4-dfd5-4a8f-b067-7b4810a586b0.png)


6. fork해서 가져온 내 본 계정 저장소에 add commit push를 한다.

    ![image](https://user-images.githubusercontent.com/51036842/164958303-ab6bb4d9-5a1c-4175-a2db-6689968d62b4.png)


    여기서 origin 이 본 계정 저장소의 별명이다.

    <span style="color:yellow">원래 명령어</span>

    ```
    git add .
    git commit -m "firstcommit for PR"
    git push <저장소 별명> <branch명>
    ```
    <span style="color:green">내 명령어</span>

    ```
    git add .
    git commit -m "firstcommit for PR"
    git push origin develop
    ```

7. 그러면 fork 해서 가져온 내 본 계정 저장소에 pull request 버튼을 눌린다.

    compare & pull request 초록 버튼이 생겼다.

    ![image](https://user-images.githubusercontent.com/51036842/164958341-883ab4b1-e2f0-4c3f-b4fb-2baa9779c581.png)

    ![image](https://user-images.githubusercontent.com/51036842/164958385-2390decf-3f82-4ffb-8c73-3581e975c76c.png)


8. pr 등록이 되었다.

    ![image](https://user-images.githubusercontent.com/51036842/164958415-110a1326-c38c-4abf-a882-3cedf669f67b.png)


9. 이제 부 계정에서 pr 내용을 확인하고 pull request를 받아본다. 

    ![image](https://user-images.githubusercontent.com/104291042/164958559-023ef367-3409-4069-82a0-4e2fea8bc124.png)

    pr이 와있다.

    merge 하기 전의 상태와 merge 후 상태를 비교해보자

    ![image](https://user-images.githubusercontent.com/104291042/164958584-24fa23c6-3fb9-41f4-96c8-f55db96802d3.png)

    부계정 저장소의 초기 상태이다.

    ![image](https://user-images.githubusercontent.com/104291042/164958601-27a7c3d1-bdcb-4644-ab1e-47157b051ad4.png)

    본 계정으로부터 pr이 와있다.

    ![image](https://user-images.githubusercontent.com/104291042/164958829-c635c1c1-b422-455c-8682-c4e44ca58eb1.png)

    merge 를 해준다.

    ![image](https://user-images.githubusercontent.com/104291042/164958840-9fc2b1bb-1f89-46cb-b859-ae533dfb2e0a.png)


    ![image](https://user-images.githubusercontent.com/104291042/164958875-b633ac44-8a9e-404c-88de-791f3207dc44.png)


    본 계정에서 보낸 full request 가 merge 가 완료되었다.


10. 내 main branch에 pull 받고 만들었던 develop branch를 삭제합니다.


    <span style="color:yellow">원래 명령어</span>

    ```
    git pull <remote> <branch>
    ```

    <span style="color:green">내 명령어</span>

    ```
    git pull prSS main
    ```

    ![image](https://user-images.githubusercontent.com/104291042/164959316-226c7102-95df-44ae-b7cc-0017e2a94476.png)

    브런치 삭제


    <span style="color:yellow">원래 명령어</span>

    ```
    git branch -D <삭제할 branch 명>
    ```

    <span style="color:green">내 명령어</span>

    ```
    git branch -D develop
    ```

    ![image](https://user-images.githubusercontent.com/104291042/164959186-82fd65f0-5162-4b01-9739-56b08a92c05f.png)

    진짜 끝 ~

---


### 추가로 한번 더 보내기


11. 이제는 remote나 다른 설정 필요 없이 내 본 계정 저장소에서 branch 만들고 add commit push 한 다음 pull request를 보내면 된다.

    <span style="color:green">내 명령어</span>
    ```
    git checkout -b develop2
    ```

    ![image](https://user-images.githubusercontent.com/104291042/164959430-bdb20622-d309-49db-862b-d6dd7fb96ed8.png)

    <span style="color:green">내 명령어</span>
    ```
    git add .
    git commit -m "second commit"
    git push origin develop2
    ```

    ![image](https://user-images.githubusercontent.com/104291042/164959517-1d32ceb5-7feb-4d11-a004-85f62f41fb07.png)

    부계정에서 pull request를 받으면 다음과 같이 merge가 되었다.

    ![image](https://user-images.githubusercontent.com/104291042/164959616-26ed2110-1ec5-480a-ad17-8335c7dcc603.png)

    remote 로부터 pull 받는다.
    ![image](https://user-images.githubusercontent.com/51036842/164959733-f969237b-5eb7-4cd5-80da-6a6c2e019a29.png)

    내 원본 저장소 (pull 받았으니 저장되어 있다.)
    ![image](https://user-images.githubusercontent.com/51036842/164959771-fb69c7ca-a8c9-4f76-87e9-62c2ad2bed5b.png)

---
### 🎁 추가 정보
<br>

### 🎨collaborator의 full request

collaborator가 full request를 보내면 보낸 사람 포함  다른 collaborator 들이 approve 및 code review를 하고, merge 할 수 있다.

### 🎨merge의 3가지 옵션

merge 하는 것에는 3가지 옵션이 있다.

![image](https://user-images.githubusercontent.com/104291042/164958631-4a1a166a-15eb-4116-b76e-d754dcffda35.png)



```
create a merge commit
squash and merge
rebase and merge
```

간단하게만 정리하면

- create a merge commit 은 merge는 2개의 커밋이 있을 때 모든 커밋이 결과적으로 시간 순서대로 병합이 되며 충돌이 있다면 해당 충돌을 해결하는 커밋이 하나더 추가되어지게 됩니다.

- squash and merge는 merge할 때 여러 개의 커밋을 하나의 커밋으로 합친 후 merge 하는 방식이고

- rebase 는 커밋의 시간에 관계없이 마지막 merge 되는 branch의 commit을 가장 뒤에 붙이는 것이고 force merge가 필수적으로 이루어집니다.



이는 다음 블로그에서 한번 읽어보자
[참고 블로그](https://sabarada.tistory.com/196)


### 👓이건 아직 검증안됨 테스트 중 
6. 만약 collaborator로 지정되어 있는 상태라면 원본에 바로 add commit push를 한다.


![image](https://user-images.githubusercontent.com/51036842/164958219-27b4c4f7-afc3-4249-9c54-19f1fd0fcda9.png)

```
git add .
git commit -m "first commit for PR"
git push origin prSS develop
```

7. 아니라면 깃허브