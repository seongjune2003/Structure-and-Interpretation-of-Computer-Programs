// SICP (Structure and Interpretation of Computer Programs)
    // Primitives, means of combination, functional abstraction, naming, and conventions for using a universal data structure in specialized ways by drawing distinctions: these are the fundamental building blocks of a good programming language. From there, imagination and good engineering judgment based on experience can do the rest.
    // The source of the exhilaration associated with computer programming is the continual unfolding within the mind and on the computer of mechanisms expressed as programs and the explosion of perception they generate.
    // For all its power, the computer is a harsh taskmaster. Its programs must be correct, and what we wish to say must be said accurately in every detail. As in every other symbolic activity, we become convinced of program truth through argument. Lisp itself can be assigned a semantics (another model, by the way), and if a program's function can be specified, say, in the predicate calculus, the proof methods of logic can be used to make an acceptable correctness argument. Unfortunately, as programs get large and complicated, as they almost always do, the adequacy, consistency, and correctness of the specifications themselves become open to doubt, so that complete formal arguments of correctness seldom accompany large programs. Since large programs grow from small ones, it is crucial that we develop an arsenal of standard program structures of whose correctness we have become sure—we call them idioms—and learn to combine them into larger structures using organizational techniques of proven value
    
    // 프로그램의 전반적인 과정
    // 사람이나 물건에 '이름'이 있고 그 이름을 들었을 때 관련된 내용이 자연스럽게 머리속에서 떠오르듯이 컴퓨터도 각각의 기능에 이름을 붙임으로서 좀 더 풍요로운
    // 프로그램 언어 생활이 가능해지는 것. 
    // 생각을 정리하고 이걸 프로그램화하는데 이게 나중에 데이터 양이 많아지고(프로그램이 커지면) 정확성이나 유지보수성에서 문제가 생기지. 이걸 해결하기 위해서
    // 더 큰 자료구조에 집어넣어서 관리를 한다 이말임. 이러한 전반적인 과정에 대해 정리한게 이 책의 전반적인 내용.
    // The most important of these is the central role played by different approaches to dealing with time in computational models: objects with state, concurrent programming, functional programming, lazy evaluation, and nondeterministic programming. 
    
    //  State, 함수형 프로그래밍, 병행 컴퓨팅, 비결정론적 알고리즘, 느긋한 계산법
    // State  =  Variables in programming refer to storage location that can contain values. These values can be changed during runtime. The variable can be used at multiple places within code and they will all refer to the value stored within it.
    // 
    
    // Our design of this introductory computer-science subject reflects two major concerns. First, we want to establish the idea that a computer language is not just a way of getting a computer to perform operations but rather that it is a novel formal medium for expressing ideas about methodology. Thus, programs must be written for people to read, and only incidentally for machines to execute. Second, we believe that the essential material to be addressed by a subject at this level is not the syntax of particular programming-language constructs, nor clever algorithms for computing particular functions efficiently, nor even the mathematical analysis of algorithms and the foundations of computing, but rather the techniques used to control the intellectual complexity of large software systems.
    // Our goal is that students who complete this subject should have a good feel for the elements of style and the aesthetics of programming. They should have command of the major techniques for controlling complexity in a large system. They should be capable of reading a 50-page-long program, if it is written in an exemplary style. They should know what not to read, and what they need not understand at any moment. They should feel secure about modifying a program, retaining the spirit and style of the original author.
    
    // Computation provides a framework for dealing precisely with notions of how to.
    // 신텍스 구조나 알고리즘이나 그런게 중요한게 아니라 우리가 머리속으로 생각하고 결정하는 그런 인지적 과정이 컴퓨터에서는 어떻게 작동하는지 그런 플로우를 
    // 찾는 것이 이 책의 핵심인거 같네.


    // 1강   Building Abstractions with Procedures Functions
    // 모듈러 설계
    //  Well-designed computational systems, like well-designed automobiles or nuclear reactors, are designed in a modular manner, so that the parts can be constructed, replaced, and debugged separately.
    // 
    
    // primitive expressions, which represent the simplest entities the language is concerned with,
    // means of combination, by which compound elements are built from simpler ones, and
    // means of abstraction, by which compound elements can be named and manipulated as units.
    
    // 제일 작은 의미단위(1)에서 조합하고(1) 그 조합한걸 다시 조합재료 유닛으로 쓴다(3)
    // 데이터 -> 우리가 조종하려는 대상
    // 프로시저  -> 그 대상을 조종하는 규칙. 룰.
    
    // 이름과 연관되는 의미(memory)가 있도록 짝짓는 것. 여기서 memory를 environment라고 한다.
    
    // Squaring    "To square something, take it times itself"
    // substitution model
    // 함수나 substitution model이나 이런게 어떻게 작동하는지는 여기서 중요한게 아님.
    // 이게 처음엔 그냥 간단하게 표현할 수 있는 데이터가 나중에 복잡해지면서 기존의 방식이 부적적해지고 그에 따라 새로운 방법을 찾고 이런 인지 흐름을 파악하는게 중요한 것
    // 예를 들어서 (이 책의 표현대로) 각각의 Name별로 그에 맞는 Value가 있다고 생각해보자고.
    // '음식'이라는 단어를 생각했다고 생각했을 때, '음식'이라는 Name에 해당하는 Value는 여러가지가 있겠지
    // 예를 들어서 음식의 종류에 따라서 한식, 양식, 일식, 중식 이렇게 연상되는 것들이 있을 수 있을테고
    // 한식이면 불고기, 비빕밥, 간장게장.   양식이면 리소토, 파스타, 스테이크,  일식이랑 중식도 비슷하게... 이렇게 연상되는 것이 있을거 아니야
    // 만약 데이터가  음식 - 음식종류(한,양,일,중)  이렇게만 있으면 간단한 방법으로도 표현할 수 있겠지만
    // 예를 들어서 위에   음식 - 음식종류 - 음식메뉴 여기에서 하나 더 추가해서
    // 음식 - 음식종류 - 음식의 맛(짠맛, 단맛...) - 음식메뉴 이렇게까지 추가한다면 하나의 데이터를 찾으려 해도 트리를 상당히 많이 타야겠지.
    // 1) 어느 음식 종류에 속하는가?   2) 음식의 맛이 무엇인가?  3) 음식의 이름은?  이렇게만 해도 3번의 트리를 타야하고.
    // 이럴때 만약 1번과 2번만 리스트로 바꾸고 각 Index에 맞는 Value를 지정하는 구조로 바꾸면 이러한 과정이 훨씬 간단해질 수 있겠지.
    // 책에서 말했던 "데이터가 복잡해지면 그에 따라 적합한 자료구조의 형태도 바뀐다"는 거가 이런게 아닌가 싶다.
