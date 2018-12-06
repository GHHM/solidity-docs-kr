############
기여하기
############

도움은 언제나 감사합니다!

시작하려면 먼저 Solidity의 구성 요소와 빌드 프로세스에 익숙해지기 위해 building-from-source(https://solidity.readthedocs.io/en/v0.5.1/installing-solidity.html#building-from-source)를 시도해보세요.
를 직접 확인하십시오. 또한 Solidity에서 스마트 컨트렉트 작성에 능숙해지는데 유용합니다.

특히, 우리는 다음의 영역에서 도움이 필요합니다:

* 다큐멘테이션 개선
* 'StackExchange(https://ethereum.stackexchange.com)와 Solidity Gitter(https://gitter.im/ethereum/solidity)에서 다른 사용자들의 질문에 답변하기
* Solidity 깃허브 이슈(https://github.com/ethereum/solidity/issues)문제 수정 및 답변. 특히 'up-for-grabs(https://github.com/ethereum/solidity/issues?q=is%3Aopen+is%3Aissue+label%3Aup-for-grabs)' 
    외부 참여자들의 입문 이슈를 의미합니다.
  which are
  meant as introductory issues for external contributors.

How to Report Issues
====================

To report an issue, please use the
`GitHub issues tracker <https://github.com/ethereum/solidity/issues>`_. When
reporting issues, please mention the following details:

* Which version of Solidity you are using
* What was the source code (if applicable)
* Which platform are you running on
* How to reproduce the issue
* What was the result of the issue
* What the expected behaviour is

Reducing the source code that caused the issue to a bare minimum is always
very helpful and sometimes even clarifies a misunderstanding.

Workflow for Pull Requests
==========================

In order to contribute, please fork off of the ``develop`` branch and make your
changes there. Your commit messages should detail *why* you made your change
in addition to *what* you did (unless it is a tiny change).

If you need to pull in any changes from ``develop`` after making your fork (for
example, to resolve potential merge conflicts), please avoid using ``git merge``
and instead, ``git rebase`` your branch.

Additionally, if you are writing a new feature, please ensure you write appropriate
Boost test cases and place them under ``test/``.

However, if you are making a larger change, please consult with the `Solidity Development Gitter channel
<https://gitter.im/ethereum/solidity-dev>`_ (different from the one mentioned above, this on is
focused on compiler and language development instead of language use) first.


Finally, please make sure you respect the `coding standards
<https://raw.githubusercontent.com/ethereum/cpp-ethereum/develop/CodingStandards.txt>`_
for this project. Also, even though we do CI testing, please test your code and
ensure that it builds locally before submitting a pull request.

Thank you for your help!

Running the compiler tests
==========================

Solidity includes different types of tests. They are included in the application
called ``soltest``. Some of them require the ``cpp-ethereum`` client in testing mode,
some others require ``libz3`` to be installed.

To disable the z3 tests, use ``./build/test/soltest -- --no-smt`` and
to run a subset of the tests that do not require ``cpp-ethereum``, use ``./build/test/soltest -- --no-ipc``.

For all other tests, you need to install `cpp-ethereum <https://github.com/ethereum/cpp-ethereum/releases/download/solidityTester/eth>`_ and run it in testing mode: ``eth --test -d /tmp/testeth``.

Then you run the actual tests: ``./build/test/soltest -- --ipcpath /tmp/testeth/geth.ipc``.

To run a subset of tests, filters can be used:
``soltest -t TestSuite/TestName -- --ipcpath /tmp/testeth/geth.ipc``, where ``TestName`` can be a wildcard ``*``.

Alternatively, there is a testing script at ``scripts/test.sh`` which executes all tests and runs
``cpp-ethereum`` automatically if it is in the path (but does not download it).

Travis CI even runs some additional tests (including ``solc-js`` and testing third party Solidity frameworks) that require compiling the Emscripten target.

Whiskers
========

*Whiskers* is a templating system similar to `Mustache <https://mustache.github.io>`_. It is used by the
compiler in various places to aid readability, and thus maintainability and verifiability, of the code.

The syntax comes with a substantial difference to Mustache: the template markers ``{{`` and ``}}`` are
replaced by ``<`` and ``>`` in order to aid parsing and avoid conflicts with :ref:`inline-assembly`
(The symbols ``<`` and ``>`` are invalid in inline assembly, while ``{`` and ``}`` are used to delimit blocks).
Another limitation is that lists are only resolved one depth and they will not recurse. This may change in the future.

A rough specification is the following:

Any occurrence of ``<name>`` is replaced by the string-value of the supplied variable ``name`` without any
escaping and without iterated replacements. An area can be delimited by ``<#name>...</name>``. It is replaced
by as many concatenations of its contents as there were sets of variables supplied to the template system,
each time replacing any ``<inner>`` items by their respective value. Top-level variables can also be used
inside such areas.
