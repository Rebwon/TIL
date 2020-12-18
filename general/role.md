## [권한 설계](https://www.whiteship.me/-ea-b6-8c-ed-95-9c-3-eb-8b-a8--ea-b5-ac-ec-a1-b0/)

Member -> 1 Role -> 1 Right

하나의 사용자는 한 개의 역할이 있고 한 개의 권한이 존재한다.

이런 설정에 따라 사용자와 관리자를 구분한다면, ROLE_MEMBER, ROLE_ADMIN 밖에는 존재하지 않는다.

저런 단편적인 구조에서 예컨대, 다음과 같은 시나리오가 추가되면 머리가 아파진다.

"일반 회원 중에서 관리자는 아니지만 스터디를 관리할 수 있으면 좋겠다"

위와 같은 시나리오를 만족하려면 다음과 같이 해야 한다.

1. StudyManager라는 Role을 만듭니다.
2. 해당 사용자에게 StudyManager Role을 추가해줍니다.
3. URL 설정(여기서의 URL은 리퀘스트 URL) 및 메서드 보안 설정을 찾아다니며 hasRole(Role_StudyManager)를 추가해줘야 합니다.

만약에 URL 설정이 여러줄이고 메서드 보안이 여러줄이라면 어떻게 될까요??
이런 상태에서 권한이라는 것은 의미가 있기나 할까요?

그럼, URL 설정을 Right(권한) 기반으로 바꿨다고 가정하겠습니다. 아마 지금보다 더 세부적으로 URL 권한을 설정할테니 갈아타는데 손이 좀 갈 것으로 예상됩니다.

전부 Role 기반으로 바꿨다는 가정하에 다시 한 번 위의 시나리오를 적용하는 과정을 상상해 보겠습니다.

1. StudyManager라는 Role을 만들고
2. 해당 Role에 Add_Study, Update_Study, Delete_Study, End_Study, Start_Study 등 Study와 관련된 모든 권한을 추가해둡니다.
3. 해당 사용자에게 StudyManager 권한을 줍니다.

StudyManager라는 Role을 만드는 과정이 다소 귀찮을 순 있겠지만, A라는 사용자 말고 B 사용자에게 위와같은 시나리오를 적용해야 하는 상황이 온다면 어떨까요? 1, 2번은 필요없고 3번만 하면 됩니다.

“스터디 관리자여도 스터디 삭제는 못하게 하고 싶다. 그건 오직 관리자만 할 수 있게해야지”

이런 시나리오를 적용해야 한다면 Role 기반의 설정을 사용할때는 분명 study/34/delete.do URL이나 deleteStudy(study) 메서드에 hasRole(Role_StudyManager, Role_Admin)이라고 되어있을텐데, 저걸.. 다시 hasRole(Role_ADMIN)으로 바꿔줘야 할 겁니다.

그런데 Right기반으로 설정되어 있는 경우 hasRole(Right_Delete_Study)라고 설정되어 있을것이기 때문에 건드릴건 아무것도 없고, 단지 Role_StudyManager에서 Right_Delete_Study 만 빼주면 됩니다.