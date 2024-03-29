
### Transaction이란??

	트랜잭션이란 - 데이터베이스의 상태를 변화시키기 위해 수행하는 작업 단위를 뜻한다.
	데이터 베이스의 상태를 변화시키는 것은 무엇을 의미하는가?
	바로 질의어 
		SELECT, Update, Insert, Delete 등을 통해 데이터베이스에 접근하는 것이다.

**착각하기 쉬운점은 작업의 단위는 하나의 질의 문장이 아니라는 점!!!

```
작업단위는 많은 질의어 명령문들을 개발자가 정하는 기준에 따라 정하는 것을 의미한다!

예를들어 게시판에서
사용자가 게시글을 작성하고, 작성하기 버튼을 상호작용하고 업데이트 된 게시판을 보게 되었을때

데이터베이스에서 일어나는 일은
Post Request가 가고 서버 DB에 게시글이 저장되고 다시 클라이언트로 Response되어 Redirection(Select)이 일어나서 Update가 된다.

여기서 일어나는 작업은 Insert와 Select가 함께 일어난다

이런 일련의 과정 설계를 Transaction이라고 한다.

트랜잭션 설계를 처음부터 잘해야 효율성 있는 시스템을 개발할 수 있다.

```


---

## Transaction의 4가지 특징

*ACID를 잘 지키자*

*Atomicity(원자성)* - **원자성은 Transaction이 DB에 전부 반영되거나 전부 반영되지 않아야 한다는 특성이다. 트랜잭션은 사람이 설계한 논리적인 작업 단위기 때문에 일처리가 작업단위 별로 일어나야한다.

*Consistency(일관성) - **작업처리 결과가 항상 일관성이 있어야 한다는 것 트랜잭션이 일어날 때 다른 트랜잭션으로 인해 DB가 변경되더라도 트랜잭션이 일어났을 당시에 DB를 참조하여 진행해야 한다. 이렇게 함으로써 사용자가 일관된 데이터를 볼 수 있게 만들어야 한다.

*Isolation(독립성) - **둘 이상의 트랜잭션이 일어나고 있을 때 서로 간섭하거나 침범해서는 안되는 성질이다. 트랜잭션이 끝날때까지 다른 트랜잭션이 참조할 수 없다

*Durability(지속성) - **성공적인 트랜잭션이 일어나면 트랜잭션의 결과가 영구적으로 시스템에 반영되어야 하는 성질이다.

