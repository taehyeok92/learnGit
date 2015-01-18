#기본

`git init`
`git clone 저장소주소 폴더명`

지정한 커밋의 로그메세지를 수정
`git commit -C HEAD -a --amend`

변경내용 되돌리게..
`git checkout -- 파일명`

인덱스에 스테이징한거 취소(add한거)
`git reset HEAD <file>`

터미널에서 컬러로 나오게
`git config --global color.ui "true";`

환경변수보기
`git config --global --list`


#Stash
unstaged상태의 파일을 일시적으로 백업하고 워킹디렉토리를 깨긋하게유지(아직 커밋하면 안되는 작업중에 pull받아야하는데 그때 충동이 일어날경우…작업도 안됬는데 중간에 커밋하고 풀하는건 안좋다 이때 stash사용)
`git stash`
`git stash pop //복구`
`git stash apply //스태시에서 제거하지않고 복구`
`git stash list`
`git stash show stash이름`


#브랜치

브랜치 조회
`git branch`

새브랜치
`git branch 브랜치명`

브랜치 삭제
`git branch -d 브랜치명`

브랜치 이름변경
`git branch -m 존재하는브랜치명 새로운브랜치명`

태그붙이기
`git tag 태그명 브랜치명`

해당브랜치나 태그로 체크아웃
`git checkout 브랜치명/태그명`

해당 브랜치를 현재 브랜치로 합침
`git merge 브랜치명`


해당 브랜치의 변경사항을 현재 브랜치에 적용
`git rebase 브랜치명`


#로그관리
해당 파일의 마지막 변경 상세 이력을 볼수있다.
`git blame 파일명`

이전 커밋 취소
`git reset 커밋명`
`git reset HEAD^ //최근1개의 커밋취소`
`git reset --hard HEAD 변경사항무시 마지막커밋 복구`

커밋상세 정보를 보여준다.
`git show 커밋주소`


# 소스비교
영역 구분
워킹 트리 working tree (workspace)
인덱스 index (planned tree, staging area)
로컬 브랜치(현재 브랜치) local branch[local repository]
리모트 트래킹 브랜치 remote tracking branch[local repository] : 리모트 브랜치를 로컬에서 참조가능(fetch 명령으로 갱신)
리모트 브랜치 remote branch[remote repository]

1. 워킹 트리 → 인덱스
`git status` : 워킹트리에서 아직 인덱스에 add되지 않은 파일내역
`git diff` : 워킹 트리와 인덱스간 소스내용 차이점
`git diff <파일>` : 해당 파일에 대한 워킹트리와 인덱스간 소스내용 차이점

2. 인덱스 → 로컬 브랜치
`git status` : 인덱스에서 아직 commit되지 않은 파일내역
`git diff --cached` : 인덱스와 로컬브랜치(HEAD) 간의 차이점. "commit" 명령을 수행할 경우 반영되는 내용들

3. 워킹 트리 → 로컬 브랜치
`git diff HEAD` : 워킹 트리와 로컬 브랜치(HEAD)의 소스내용 차이. 마지막 commit 이후 변경사항. 
                     "commit -a" 명령을 수행할 경우 반영되는 내용들
`git diff HEAD -- <파일>` : 해당 파일에 대한 워킹트리와 마지막 commit 이후의 소스내용 차이점

4. 워킹 트리 → 리모트 브랜치
`git diff FETCH_HEAD` : 워킹 트리와 리모트 브랜치(FETCH_HEAD)의 소스내용 차이점
`git diff FETCH_HEAD -- <파일>` : 해당 파일에 대한 워킹 트리와 리모트 브랜치 소스내용 차이점

5. 로컬 브랜치 → 리모트 브랜치 : `Outgoing Changes`
`git log FETCH_HEAD..` : 로컬 브랜치에서 리모트 브랜치에 반영할 변경내역(커밋기록)
`git diff FETCH_HEAD...` : 로컬 브랜치에서 리모트 브랜치에 반영할 소스 변경내용 

6. 리모트 브랜치 → 로컬 브랜치 : `Incoming Changes`
`git log ..FETCH_HEAD` : 리모트 브랜치에서 로컬 브랜치로 가져와야할 변경내역(커밋기록)
`git diff ...FETCH_HEAD` : 리모트 브랜치에서 로컬 브랜치로 가져와야할 소스 변경내용



#원격저장소

원격저장소의 변경사항을 가져와서 브랜치를 갱신
`git fetch`

새로운 원격저장소 추가
`git remote add 이름 저장소주소`

원격 저장소 목록확인
`git remote`

해당 원격저장소의 정보를 볼 수 있습니다.
`git remote show 이름`

원격저장소를 제거합니다.
`git remote rm 이름`

개별파일 원복
`git checkout  -- <파일>` : 워킹트리의 수정된 파일을 index(staging area)에 있는 것으로 원복
`git checkout HEAD -- <파일명>` : 워킹트리의 수정된 파일을 HEAD에 있는 것으로 원복(이 경우 --는 생략가능)
`git checkout FETCH_HEAD -- <파일명>` : 워킹트리의 수정된 파일의 내용을 FETCH_HEAD에 있는 것으로 원복? merge?(이 경우 --는 생략가능)
 
index 추가 취소
`git reset -- <파일명>` : 해당 파일을 index(staging area)에 추가한 것을 취소(unstage). 워킹트리의 변경내용은 보존됨. (--mixed 가 default)
`git reset HEAD <파일명>` : 위와 동일
 
commit 취소
`git reset HEAD^` : 최종 커밋을 취소. 워킹트리는 보존됨. (커밋은 했으나 push하지 않은 경우 유용)
`git reset HEAD~2` : 마지막 2개의 커밋을 취소. 워킹트리는 보존됨.
`git reset --hard HEAD~2` : 마지막 2개의 커밋을 취소. index 및 워킹트리 모두 원복됨.
`git reset --hard ORIG_HEAD` : 머지한 것을 이미 커밋했을 때,  그 커밋을 취소. (잘못된 머지를 이미 커밋한 경우 유용)
`git revert HEAD` : HEAD에서 변경한 내역을 취소하는 새로운 커밋 발행(undo commit). (커밋을 이미 push 해버린 경우 유용)
 
워킹트리 전체 원복
`git reset --hard HEAD` : 워킹트리 전체를 마지막 커밋 상태로 되돌림. 마지막 커밋이후의 워킹트리와 index의 수정사항 모두 사라짐. 
                                  (변경을 커밋하지 않았다면 유용)
 
 
* 참조 : reset 옵션
--soft : index 보존, 워킹트리 보존. 즉 모두 보존.
--mixed : index 취소, 워킹트리만 보존 (기본 옵션)
--hard : index 취소, 워킹트리 취소. 즉 모두 취소..

* 출처 : http://yonoo88.tistory.com/204