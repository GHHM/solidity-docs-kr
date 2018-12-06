

.. _contract_structure:

***********************
콘트랙트 구조
***********************

솔리디티의 콘트렉트는 객체 지향 언어의 클래스와 비슷하다
각각의 콘트렉트는 상태 변수, 함수, 함수 모디파이어, 이벤트, 구조체타입과 이넘타입을 선언할 수 있다.
뿐만 아니라 콘트렉트는 다른 콘트렉트를 상속할 수 있다.

.. _structure-state-variables:

상태변수
===============

상태 변수는 콘트렉트 저장소에 영속적으로 저장되는 값이다.

::

    pragma solidity ^0.4.0;

    contract SimpleStorage {
        uint storedData; // 상태 변수
        // ...
    }

타입(https://solidity.readthedocs.io/en/develop/types.html#types)섹션에서 유효한 변수 타입을,
접근제한자 및 한정자(https://solidity.readthedocs.io/en/develop/contracts.html#visibility-and-accessors)
섹션에서 쓸 수 있는 접근제한성에 대한 자세한 내용을 살펴볼 수 있다.

.. _structure-functions:

함수
=========

함수는 콘트렉트 내에서 실행 가능한 코드 단위다.

::

    pragma solidity ^0.4.0;

    contract SimpleAuction {
        function bid() public payable { // 함수
            // ...
        }
    }

함수호출은 내부 혹은 외부에서 호출될 수 있으며
콘트렉트에 대해서 각기 다른 접근제한성(https://solidity.readthedocs.io/en/develop/contracts.html#visibility-and-accessors)을 가지고 있다.

.. _structure-function-modifiers:

함수 변환자
==================

함수 조건자는 함수의 의미(semantic)를 선언적(declarative) 방식으로 수정할 수 있다.
(콘트렉트의 함수 조건자 https://solidity.readthedocs.io/en/develop/contracts.html#modifiers 참조)

::

    pragma solidity ^0.4.11;

    contract Purchase {
        address public seller;

        modifier onlySeller() { // 제한자
            require(msg.sender == seller);
            _;
        }

        function abort() public onlySeller { // 모디파이어 사용
            // ...
        }
    }

.. _structure-events:

이벤트
======

이벤트는 이더리움 가상머신(EVM)이 손쉽게 로깅할 수 있는 도구로써 사용되는 편리한 인터페이스다.

::

    pragma solidity ^0.4.21;

    contract SimpleAuction {
        event HighestBidIncreased(address bidder, uint amount); // 이벤트 선언

        function bid() public payable {
            // ...
            emit HighestBidIncreased(msg.sender, msg.value); // 이벤트 발생
        }
    }

이벤트(https://solidity.readthedocs.io/en/develop/contracts.html#events) 파트에서 이벤트가 어떻게 선언되고 분산어플리케이션에서 어떻게 사용되는지 확인할 수 있다.

.. _structure-struct-types:

구조체
=============

구조체는 여러 변수를 가지는 사용자가 정의한 타입이다.(https://solidity.readthedocs.io/en/develop/types.html#structs)

::

    pragma solidity ^0.4.0;

    contract Ballot {
        struct Voter { // 구조체
            uint weight;
            bool voted;
            address delegate;
            uint vote;
        }
    }

.. _structure-enum-types:

열거형
==========

열거형(Enums) 은 유한개의 집합중 한 가지가 선택될 수 있는 커스텀 타입이다.(https://solidity.readthedocs.io/en/develop/types.html#enums)

::

    pragma solidity ^0.4.0;

    contract Purchase {
        enum State { Created, Locked, Inactive } // 열거형
    }
