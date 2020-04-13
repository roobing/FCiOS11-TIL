vi에서 #는 주석처리이다.(단, #하고 한칸 띄워서 쓰게 되면 h1이 됨)w

github에서 저장소 만드는 것 = remote repository 생성

git init 하는 것 = index, local repositoy 생성

git remote add 하는 것 = remote 저장소에 별명을 붙이는 것

git commit 하는것 = index -> local repository

Commit 시 제목(말머리)만으로 내용을 짐작할 수 있게 작성해야함

제목(말머리) 종류

1. docs: documentations
2. conf: configurations
3. feat: features
4. bugfix: fix bug

git push -u repository별명 master 하는것 = 내가 별명으로 지어놓은 저장소의 master branch에 택배를 보내겠다. 

commit 시 동작 단위로 쪼개서 커밋하는게 좋다.

.gitignore : 

keyfile.pem은 반드시 포함시켜야함. 클라우드 채굴장으로 끌려간다.

gitignore.io 에서 생성하는 .gitignore파일을 사용하는 것이 git에서 기본적으로 만들어주는 것보다 좋다.



주제/{date}-{subject}.md 형식의 파일 이름이 좋다.

git status -uall 변경사항이 발생한 폴더 안의 내용까지 보여준다.

homebrew : macOS package manager

npm: node.js package manager

해당 프로그램이 깔려있는지 확인하려면 터미널에서 프로그램명 -v 쳐봐서 나오면 깔린거임

ex) npm -v

package.json 

터미널에서 %표시가 없으면 어떤 프로그램이 실행중인거임. 꺼줘야함. 컨트롤 씨

__config.yml : html의 meta 태크에 들어가는 내용이라고 보면됨.

\# Deployment -> 배포 정보

\## Docs: https://hexo.io/docs/deployment.html

deploy:

 type: git

 repo: https://github.com/roobing/roobing.github.io.git



generate vs deploy

gen은 서버를 띄워서 하고자 할때(local)

deploy github 서버에 적용시킬려고



post를 작성한 후에는 반드시 gen & deploy 해줘야함



cd 꿀팁

cd 하위폴더1 입력후 탭 누르면 하위폴더1의 하위폴더들을 보여주고 하위폴더2 하고 탭 하면 또 그 하위 폴더들을 보여주고 쭉쭉쭉

