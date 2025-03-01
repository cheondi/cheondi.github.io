---
layout: post
title: Singleton 패턴
tags: [unity, vscode,designPatterns]
categories: [designPatterns]
comments : true
---
<br>
<br>
<br>
<br>

# Singleton 패턴

<BR>

## 1.서버 관련 함수 Singleton으로 관리

<br>

디자인 패턴 중 생성 패턴 중 하나인 Singleton 패턴을 이용하여 서버 관련 메서드를 관리한다. Singleton 패턴은 클래스의 인스턴스는 오직 하나임을 보장하며 이 인스턴스에 접근할 수 있는 방법을 제공한다.

<br>

## 1.1 사용 동기
어떤 클래스 경우에는 정확히 하나의 인스턴스만을 갖도록 하는 것이 중요하다. 예를 들어 파일 시스템의 경우에 윈도우 매니저는 하나여야 하고, 한 회사에는 하나의 회계 시스템만 운영되는 것 등 클래스에서 만들 수 있는 인스턴스가 오직 하나일 경우에 이에 대한 접근은 어디에서든지 하나로만 통일하여 제공하여야 한다.

<br>

## 1.2 사용 시기

* 클래스의 인스턴스가 오직 하나여야 함을 보장하고, 잘 정의된 접근 방식에 의해 모든 클라이언트가 접근할 수 있도록 해야 항 때
* 유일하게 존재하는 인스턴스가 상속에 의해 확장되어야 할 때, 클라이언트는 코드의 수정 없이 확장된 서브클래스의 인스턴스를 사용할 수 있어야 할 때

## 1.3 장점

* _유일하게 존재하는 인스턴스로의 접근을 통제할 수 있다._ 싱글톤 클래스 자체가 인스턴스를 캡슐화하고 있기 때문에 이 클래스에서 클라이언트가 언제 어떻게 이 인스턴스에 접근할 수 있는지를 제어할 수 있다.

<br>

* _변수 영역을 줄인다._ Singleton 패턴은 전역 변수보다 장점이 있는데, 그것은 전역 변수를 사용해서 변수 영역을 망치는 일을 없애 준다는 것이다. 즉, 전역 변수를 정의하여 발생하는 디버깅의 어려움 등의 문제를 없애 준다.

<br>

* _오퍼레이션의 정제를 가능하게 한다._ Singleton 클래스는 상속될 수 있기 때문에, 이 상속된 서브클래스를 통해서 새로운 인스턴스를 만들 수 있다. 또한 이 패턴을 사용하면, 런타임 시 필요한 클래스의 인스턴스로 애플리케이션 합성을 변경할 수도 있다.

<br>

* _인스턴스의 개수를 변경하기가 자유롭다._ Singleton 클래스의 인스턴가 하나 이상 존재할 수 있도록 변경해야 하는 경우도 있다. 애플리케이션에 존재하는 인스턴스가 다수여야 하는 경우도 하나가 존재해야 하는 경우와 동일한 방법으로 해결하는 것이다. 즉, Singleton 클래스의 인스턴스에 접근할 수 있는 허용 범위를 결정하는 오퍼레이션만을 변경하면 된다. 왜냐하면 기존에는 하나의 인스턴스로만 접근을 허용했다면, 이제는 여러 개의 인스턴스를 생성해서 그 각각의 인스턴스로 접근할 수 있도록 오퍼레이션의 구현을 바꾸면 되기 때문이다.

<br>

*_클래스 오퍼레이션을 사용하는 것보다 훨씬 유연한 방법이다._ 싱글톤과 동일한 기능을 발휘하게 하는 방법이 클래스 오퍼레이션을 사용하는 방법이다. C++에서는 정적 멤버 함수를, 스몰토크에서는 클래스 메서드를 제공한다. 이들 두 언어 모두에서는 클래싀 인스턴스가 하나 이상 존재할 수 있도록 설계를 변경하는 것이 어렵다. C++의 정적 멤버 함수는 가상 함수일 수 없다. 함수가 되면 서브클래스들이 이 오퍼레이션을 상속하여 다형성을 지원하도록 재정의 할 수 없기 때문이다.

<br>

## 1.4 구현

<br>
<br>
1. _인스턴스가 유일해야 함을 보장한다._ Singleton 패턴은 클래스의 인스턴스가 오로지 하나임을 만족해야 한다. 가장 일반적인 방법은 인스턴스를 생성하는 오퍼레이션을 클래스 오퍼레이션으로 만드는 것이다. 이 오퍼레이션은 유일한 인스턴스를 관리할 변수에 접근해서 이 변수에 유일한 인스턴스로 초기화하고 이 변수를 되돌려 줌으로써 클라이언트가 유일한 인스턴스를 사용할 수 있도록 한다. 이 방법은 Singleton 클래스의 인스턴스가 처음 사용되기 바오 직전에 그 인스턴스를 생성하고 초기화되도록 보장한다. <br>
C++에서 클래스 오퍼레이션을 정의하는 방법은 정적 멤버 함수를 정의하는 것이다. Singleton 클래스에 `Instance()` 오퍼레이션을 정적 멤버 함수로 정의함으로써 클래스 오퍼레이션을 정의한다. Singleton 클래스는 또한 정적 멤버 변수 `_instance`를 정의하여 실제로 생성될 유일한 인스턴스에 대한 참조자를 갖도록 한다. 

<br>

Singleton 클래스는 다음과 같이 선언될 수 있다.

<br>

~~~ c++
class Singleton 
{
    public : static Singleton* Instance();
    protected : Singleton();
    private : static Singleton* _instance;
};
~~~

<br>

이에 대한 구현의 에를 보면 다음과 같다.

<br>

~~~ c++
Singleton* Singleton : _instance = 0;

Singleton* Singleton ::Instance ()
{
    if(_instance == 0)
    {
        _instance = new Singleton;
        //새로운 인스턴스 생성
    }
    return _instance;
}
~~~

<br>

클라이언트는 반드시 `Instance()`함수를 통해서만 인스턴스에 접근해야 한다. 초기에 `_instance` 멤버 변수는 0으로 초기화한다. 이는 실제로 첫 번째 접근 시도가 이루어지기 전까지는 0의 값을 갖고 있다가 실제로 접근이 이루어질 때 유일한 인스턴스를 생성하여 이 인스턴스에 대한 참조자를 관리하도록 하고 있다. <br>

그리고 주의를 기울여 볼 것은 생성자가 다른 예와 달리 `protected`로 선언되어 실제로 클라이언트가 임의로 Singleton 클래스로부터 인스턴스를 생성하여고 `new Singleton()`이라고 했다면, 컴파일 시 오류가 발생한다. 이로써 유일한 인스턴스의 관리를 가능하게 한다. <br>

`_instance`는 Singleton 객체에 대한 포인터이며, `Instance()` 함수는 이 변수에 Sinleton 클래스로부터 상속된 서브클래스의 인스턴스로 참조자를 할당할 수 있다. <br>

C++로의 구현에 한 가지 더 짚고 넘어 갈 것은, Singleton은 전역 변수나 정적 객체로 정의하고 이를 자동 초기화하는 것만으로는 충분하지 않다는 것이다. 그 이유는 다음과 같다. <br>

1. 정적 객체의 유일한 인스턴스만이 선언될 것이라는 보장을 할 수 없다. 즉, 정적 객체로부터 인스턴스를 얻는 선언문이 프로그램 다른 부분에 존재하더라도 이를 확인하고 방지할 방법이 없다는 것이다. <br>

2. 정적 초기화 시점에 모든 Singleton을 인스턴스화하기 위해 필요한 모든 정보를 갖고 있지 않을 수도 있다. 싱글톤에서는 프로그램 실행 중간에 계산에 의해서 값이 결정될 수도 있다. 그러나 정적으로 선언된 객체라면 그 내부에 정의된 변수릐 값을 첨은에만 정의할 수 있지, 나중에 다시 변경할 수는 없다. 이는 상수를 정의하는 것으로 이해하면 더 쉬울 것이다. 상수는 처음에 정의된 닶만이 유지되어야 하며, 나중에 그 값을 변경하는 것은 허용되지 않는다. <br>

3. C++에서는 전역 객체에 대한 생성자를 언제 호출하는지에 대한 명확한 순서를 컴파일러의 기본 정의에 내리고 있지 않다. 이 말은 Singleton들 사이에는 어떠한 종속성도 갖고 있지 않음을 의미한다. 만약 순서가 잘못된다면 오류가 생길 수도 있다. <br>

또한 전역 객체나 정적 객체의 또 다른 문제는 모든 싱글톤의 사용 여부와 상관 없이 일단은 생성하도록 하고 있다는 것이다. 그러나 정적 멤버 함수 기법을 이용하면 앞의 예에서 살펴본 것과 같이 실제 호출이 일어나는 시점에서 객체를 생성할 수도 있다.<br>

스몰토크에서는 유일한 인스턴스를 반환하는 함수를 Singleton 클래스에서 클래스 메서드로 구현한다. 하나의 인스턴스만을 생성하도록 보장하기 위해서 `new()` 오퍼레이션을 재정의한다. Singleton 클래스는 다음과 같은 두개의 클래스 메서드를 갖는다.  <br>

~~~ c++
    new
        self error : 'cannot create new object'

    default
        SolrInstance iNil ifTrue : [SoleInstance := super new].
        ^ SoleInstance
~~~

<br>
<br>
2. Singleton 클래스를 상속받는다. 서브클래스를 만드는 것이 중요한 게 아니라, 이 새로운 서브클래스의 유일한 인스턴스를 만들어 클라이언트가 이를 사용할 수 있도록 하는 것이다. Singleton의 인스턴스를 이들 서브클래스의 인스턴스로 초기화해야 한다. 가장 쉬운 방법은 Singleton 클래스의 `Instance()` 오퍼레이션을 사용할 때 어떤 싱글톤을 사용할지 결정하게 하는 것으로 슈퍼클래스의 Singleton을 사용할지, 서브클래스의 Singleton을 사용할지 결정한다.  

<br>

Singleton의 서브클래스를 선택하는 다른 방법은 `Instance()` 오퍼레이션의 구현을 슈퍼클래스가 아닌 서브클래스에서 하게 하는 것이다. 이것은 Singleton의 구현 클래스를 링크 시점에 결정할 수 있게 해준다. 그러나 이 방법은 링크 시에 이미 싱클톤을 결정했기 때문에 런타임 시 선택하는 것을 어렵게 한다. 서브 클래스의 선택을 유연하게 하려면 조건 분기문을 사용해야 하는데, 이는 가능한 Singleton 클래스에 대한 정보를 직접 코드에 작성하는 것이므로 모든 경우에 있어 유연한 방법이라고 볼 수는 없다. <br>

좀 더 유연한 방법은 Singleton에 대한 레지스트리를 사용하는 것이다. `Instance()` 오퍼레이션에 가능한 Singleton 클래스 집합을 정의하는 대신에 Singlton 클래스는 이 Singleton 인스턴스를 레지스트리에 이름을 갖는 인스턴스로 등록하는 것이다. <br>

레지스트리는 스트링으로 정의도니 이름을 해당 싱글톤 인스턴스로 대응한다. 다시 말하면, `Instance()` 오퍼레이션에서 싱글톤이 필요하면 레지스트리를 뒤져서 이름으로 해당 Singleton을 찾아 달라고 의뢰하면 레지스트리는 해당하는 Singleton을 찾아서 돌려준다. 이런 방식을 취하면 `Instance()` 오퍼레이션이 모든 Singleton 클래스와 이들 인스턴스를 알 필요는 없다. 

<br>

~~~ c++
class Singleton
{
    public :
        static void Register(const char* name, Singleton *);
        static Singleton* Instance();
    protected :
        static Singleton* Lookup(const char* name) ;
    private :
        static Singleton* _instance;
        static List<NameSingletonPair>* _registry;
};
~~~

<br>

`Register()` 오퍼레이션은 이름과 Singleton 클래스의 인스턴스를 등록한다. 레지스트리를 간단하게 하기 위해서, `NameSingletonPair` 객체의 리스트로 관리할 수도 있을 것이다. 각 `NameSingletonPair` 객체는 이름을 Singleton 인스턴스로 대응한다. `Lookup()` 오퍼레이션은 싱글톤 객체를 이름으로 찾는 기능의 오퍼레이션이다. 우리는 환경 변수가 원하는 Singleton의 이름을 지정한다고 가정한 것이다. <br>

이런 가정하에서의 `Instance()` 오퍼레이션은, 환경 변수를 이용하여 Singleton의 파라미터로 이름을 얻어 오고, 이를 이용하여 `Lookup()` 오퍼레이션을 호출하여 인스턴스를 얻고 있다.

<br>

~~~ c++
Singleton* Singleton :: Instance()
{
    if(_instance == 0)
    {
        const char* singletonName = getenv("SINGLETON");
        //사용자 또한 환경 변수는 시작 시에 이것을 지원한다.

        _instance = Lookup(singletonName);
        //해당하는 Singleton이 없다면 0을 반환한다.
    }
    return _instance;
}

~~~

<br>

Singleton 클래스는 한 가지 가능성은 생성자 안에서 자신을 등록한다. 예를 들어 `MySingleton`이라는 서브클래스는 다음과 같이 구현 할 수 있다.

~~~ c++
MySingleton::MySingleton()
{
    // ...
    Singleton::Register("MySingleton",true);
}
~~~

<br>

생성자 안에서 슈퍼클래스의 `Register()` 오퍼레이션을 이용하여 자신을 등록하고 있다. 물론 생성자는 누군가가 자신을 생성하지 않는 한 호출되지 않는 오퍼레이션이다. 이를 C++에서 해결하는 방법은 `MySingleton`의 인스턴스를 정적 객체로 정의한다.

~~~ c++
static MySingleton theSingleton;
~~~

<br>

이 코드는 MySingleton 클래스를 정의한 파일에서 선언하면 된다. <br>

Singleton 클래스는 더 이상 싱글톤 인스턴스 생성의 책임을 갖지 않고 대신, 시스템 전반에 걸쳐 싱글톤 인스턴스로의 유일한 접근 창구 역할만 한다. 그러나 아직 이 정적 객체 접근법 역시 잠재적 문제를 갖고 있다. 그것은 모든 Singleton 클래스를 상속한 서브클래스들의 인스턴스는 무조건 생성되어 있어야만 레지스트리에 등록될 수 있다는 것이다.

<br>
<br>
<br>
<br>
<br>
<br>

## 1.5 유니티에서의 Singleton
유니티에서 Singleton의 역할은 아래와 같다.

* 게임 시스템의 전반적으로 사용이 잦은 스크립트
* 씬 로드 시 오브젝트가 파괴되지 않고 유지
* 여러 개의 오브젝트가 접근 해야 하는 스크립트
* 단일 객체로만 있어야 하는 객체

위의 내용을 토대로 유니티 Singleton을 검색하여 보면 예제들이 많이 나온다. <br>

<br>

~~~ cs
using System.Collections; 
using System.Collections.Generic; 
using UnityEngine; 
public class GameManager : MonoBehaviour 
{ 
    public static GameManager instance = null; 
    private void Awake() 
    { 
        if (instance == null) 
        { 
            instance = this;
            DontDestroyOnLoad(gameObject);
        } 
        else 
        { 
            if (instance != this) 
                Destroy(this.gameObject);
        } 
    }
}

//출처: https://art-life.tistory.com/130 [무꼬's Art Life]
~~~

<br>

위와 같이 많은 사람들이 사용하는 Singleton이 있다. <br>
아래는 내가 작성한 코드이다. 현재 `엉망진창 가로세로` 프로젝트는 씬을 1개로 기획하고 만들기 때문에, 최대한으로 축약하여 사용하고 있다. <br>

<br>

~~~ cs
    public static BackEndManager Inst { get; private set; }
    void Awake() => Inst = this;
~~~

<br>

위와 같이 사용하여도 사용하는데는 지장이 없다. Singleton을 불러오는 방법은 아래와 같다.

<br>

~~~ cs
BackEndManager.Inst.myInfo
~~~

<br>

위와 같이 전역 변수처럼 사용되고 있다. 현재 `BackEndManager`에는 서버 관련 Method를 담고 있으며, 유저의 정보를 불러오고 Class에 담고 있어서 다른 스크립트에서도 쉽게 불러오고 갱신할 수 있다. <br>

<br>

~~~ cs
using System.Linq;
using System.Collections.Generic;
using UnityEngine;
using BackEnd;
using UnityEngine.UI;
using System;
using LitJson;
using DG.Tweening;
using Debug = UnityEngine.Debug;
public partial class BackEndManager : MonoBehaviour
{
#region Singleton 
    public static BackEndManager Inst { get; private set; }
    void Awake() => Inst = this;
#endregion
#region Class
    public class MyInfoCard
    {
        public string nickname; //닉네임
        public string ID; // id 코드
        public int stage; // 현재 stage
        public int wordCnt; // 맞힌 단어 갯수
        public string icon; // 현재 아이콘
        public List<string> clearWords = new List<string>(); // 클리어한 단어들
        public string loginType; //google or custom or (apple)
        public Dictionary<string,bool> achievements = new Dictionary<string, bool>();
        public void Get() // except clearwords 
        {
            //get my nickname & my login type
            BackendReturnObject userInfo = Backend.BMember.GetUserInfo();
            if (userInfo.IsSuccess())
            {
                JsonData userInfoJson = userInfo.GetReturnValuetoJSON()["row"];

                loginType = userInfoJson["subscriptionType"].ToString();
                nickname = userInfoJson["nickname"].ToString();
            }
            else
            {
                BackEndManager.Inst.AlarmNetworkError();
                return;
            }
            //get my gamedata(stage & wordCnt & icon  & achievements)
            var userData = Backend.GameData.GetMyData("Info", new Where(), 10);
            if (userData.IsSuccess())
            {
                JsonData userDataJson = userData.Rows()[0];

                stage = int.Parse(userDataJson["stage"][0].ToString());
                wordCnt = int.Parse(userDataJson["wordcnt"][0].ToString());
                icon = userDataJson["icon"][0].ToString();

                var keys = userDataJson["achieve"][0].Keys;
                foreach (var key in keys)
                {
                    if (achievements.ContainsKey(key))
                        achievements.Remove(key);
                    achievements.Add(key.ToString(), Boolean.Parse(userDataJson["achieve"][0][key][0].ToString()));
                }
            }
            else
            {
                BackEndManager.Inst.AlarmNetworkError();
                return;
            }
        }
        public void ClearStage(List<string> words,int stage)
        {
            //...
        }
    }
#endregion
    // 생략
}
~~~

위의 코드와 같이 `BackEndManager.Inst.myInfo.Get()`을 통해 유저 정보를 서버에서 불러올 수 있으며, 게임을 클리어했을 경우에는 `ClearStage()`를 통해 현재의 `myInfo`의 정보와 서버의 정보를 동시에 갱신시킨다.

## 1.6 고찰
사용하는데는 큰 지장이 없고 써야할 코드도 단순하여 유용하다. 하지만, 작성하다 보니 하나의 Singleton이 비대해지게 되어 스크립트가 4000 줄이 넘어가게 되는 경우가 있고, 메서드 하나를 찾는데도 오래 걸리기도 한다. <br>

하지만 코드를 세분화하여 메서드를 묶어서 관리하고, 기능별로 Singleton 스크립트를 만들어 관리하여 수월하게 관리할 수 있다.
















<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>


>이 포스팅은 `GOF의 디자인 패턴` 책을 기반으로 작성되었습니다.

>Erich Gramma, Richard Helm, Palph Johnson, John Vlissides 공저 (김정아 역)