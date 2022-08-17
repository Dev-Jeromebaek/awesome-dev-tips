# git branch 참조 업데이트

```bash
git remote update origin --prune
```

라는 명령어를 통해 github의 브랜치와 내 로컬 브랜치를 동기화 시킬 수 있음.
이와 관련해서 정확한 정보를 위해 더 찾아본 결과, 다음과 같다.

#### git remote prune

git remote prune은 리모트 브랜치의 더 이상 유효하지 않은 참조를 깨끗이 지우는 명령어.

```bash
$ git remote prune origin
$ git remote update --prune
```

#### git fetch -p

git fetch -p 명령어는 로컬 저장소를 최신 정보로 갱신(리모트 저장소와 동기화)하며 자동적으로 더이상 유효하지 않은 참조를 제거.

```bash
$ git fetch -p
From github.com:mylko72/myApp
 x [deleted]         (none)     -> origin/myApp/dev
 x [deleted]         (none)     -> origin/myApp/topic
 x [deleted]         (none)     -> origin/myApp/version2
```

#### Git 원격 저장소에서 삭제된 branch를 로컬 저장소에서 자동으로 지우기

특정한 저장소 또는 글로벌 Git 설정에 fetch할 때 삭제된 브랜치를 제거하는 기능을 활성시킴.

```bash
$ git config --global fetch.prune true
```

설정 이후 fetch 또는 pull 할 때 원격 저장소에서 삭제된 브랜치들이 로컬 저장소에서 자동으로 삭제.
