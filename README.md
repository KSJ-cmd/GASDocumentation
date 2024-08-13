# GASDocumentation
언리얼 엔진 5의 게임플레이 어빌리티 시스템 플러그인(GAS)에 대한 제 이해와 간단한 멀티플레이어 샘플 프로젝트입니다. 이는 공식 문서가 아니며, 이 프로젝트나 제 자신은 에픽 게임즈와 관련이 없습니다. 저는 이 정보의 정확성에 대해 어떠한 보증도 하지 않습니다.

이 문서의 목표는 GAS의 주요 개념과 클래스를 설명하고, 제 경험을 바탕으로 한 추가적인 해설을 제공하는 것입니다. 커뮤니티 사용자들 사이에는 GAS에 대한 많은 '부족 지식'이 있으며, 저는 제가 가진 모든 지식을 여기에서 공유하는 것을 목표로 합니다.

샘플 프로젝트와 문서는 현재 언리얼 엔진 5.3(UE5) 버전에 맞춰져 있습니다. 이 문서의 이전 언리얼 엔진 버전용 브랜치들이 있지만, 더 이상 지원되지 않으며 버그가 있거나 정보가 오래되었을 수 있습니다. 사용 중인 엔진 버전과 일치하는 브랜치를 사용해 주시기 바랍니다."

[GASShooter](https://github.com/tranek/GASShooter) is a sister Sample Project demonstrating advanced techniques with GAS for a multiplayer FPS/TPS.

The best documentation will always be the plugin source code.

<a name="table-of-contents"></a>
## Table of Contents

> 1. [Intro to the GameplayAbilitySystem Plugin](#intro)
> 1. [Sample Project](#sp)
> 1. [Setting Up a Project Using GAS](#setup)
> 1. [Concepts](#concepts)  
>    4.1 [Ability System Component](#concepts-asc)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.1.1 [Replication Mode](#concepts-asc-rm)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.1.2 [Setup and Initialization](#concepts-asc-setup)  
>    4.2 [Gameplay Tags](#concepts-gt)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.2.1 [Responding to Changes in Gameplay Tags](#concepts-gt-change)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.2.2 [Loading Gameplay Tags from Plugin .ini Files](#concepts-gt-loadfromplugin)  
>    4.3 [Attributes](#concepts-a)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.3.1 [Attribute Definition](#concepts-a-definition)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.3.2 [BaseValue vs CurrentValue](#concepts-a-value)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.3.3 [Meta Attributes](#concepts-a-meta)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.3.4 [Responding to Attribute Changes](#concepts-a-changes)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.3.5 [Derived Attributes](#concepts-a-derived)  
>    4.4 [Attribute Set](#concepts-as)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.4.1 [Attribute Set Definition](#concepts-as-definition)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.4.2 [Attribute Set Design](#concepts-as-design)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.4.2.1 [Subcomponents with Individual Attributes](#concepts-as-design-subcomponents)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.4.2.2 [Adding and Removing AttributeSets at Runtime](#concepts-as-design-addremoveruntime)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.4.2.3 [Item Attributes (Weapon Ammo)](#concepts-as-design-itemattributes)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.4.2.3.1 [Plain Floats on the Item](#concepts-as-design-itemattributes-plainfloats)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.4.2.3.2 [`AttributeSet` on the Item](#concepts-as-design-itemattributes-attributeset)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.4.2.3.3 [`ASC` on the Item](#concepts-as-design-itemattributes-asc)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.4.3 [Defining Attributes](#concepts-as-attributes)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.4.4 [Initializing Attributes](#concepts-as-init)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.4.5 [PreAttributeChange()](#concepts-as-preattributechange)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.4.6 [PostGameplayEffectExecute()](#concepts-as-postgameplayeffectexecute)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.4.7 [OnAttributeAggregatorCreated()](#concepts-as-onattributeaggregatorcreated)  
>    4.5 [Gameplay Effects](#concepts-ge)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.5.1 [Gameplay Effect Definition](#concepts-ge-definition)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.5.2 [Applying Gameplay Effects](#concepts-ge-applying)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.5.3 [Removing Gameplay Effects](#concepts-ga-removing)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.5.4 [Gameplay Effect Modifiers](#concepts-ge-mods)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.5.4.1 [Multiply and Divide Modifiers](#concepts-ge-mods-multiplydivide)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.5.4.2 [Gameplay Tags on Modifiers](#concepts-ge-mods-gameplaytags)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.5.5 [Stacking Gameplay Effects](#concepts-ge-stacking)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.5.6 [Granted Abilities](#concepts-ge-ga)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.5.7 [Gameplay Effect Tags](#concepts-ge-tags)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.5.8 [Immunity](#concepts-ge-immunity)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.5.9 [Gameplay Effect Spec](#concepts-ge-spec)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.5.9.1 [SetByCallers](#concepts-ge-spec-setbycaller)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.5.10 [Gameplay Effect Context](#concepts-ge-context)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.5.11 [Modifier Magnitude Calculation](#concepts-ge-mmc)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.5.12 [Gameplay Effect Execution Calculation](#concepts-ge-ec)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.5.12.1 [Sending Data to Execution Calculations](#concepts-ge-ec-senddata)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.5.12.1.1 [SetByCaller](#concepts-ge-ec-senddata-setbycaller)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.5.12.1.2 [Backing Data Attribute Calculation Modifier](#concepts-ge-ec-senddata-backingdataattribute)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.5.12.1.3 [Backing Data Temporary Variable Calculation Modifier](#concepts-ge-ec-senddata-backingdatatempvariable)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.5.12.1.4 [Gameplay Effect Context](#concepts-ge-ec-senddata-effectcontext)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.5.13 [Custom Application Requirement](#concepts-ge-car)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.5.14 [Cost Gameplay Effect](#concepts-ge-cost)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.5.15 [Cooldown Gameplay Effect](#concepts-ge-cooldown)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.5.15.1 [Get the Cooldown Gameplay Effect's Remaining Time](#concepts-ge-cooldown-tr)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.5.15.2 [Listening for Cooldown Begin and End](#concepts-ge-cooldown-listen)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.5.15.3 [Predicting Cooldowns](#concepts-ge-cooldown-prediction)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.5.16 [Changing Active Gameplay Effect Duration](#concepts-ge-duration)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.5.17 [Creating Dynamic Gameplay Effects at Runtime](#concepts-ge-dynamic)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.5.18 [Gameplay Effect Containers](#concepts-ge-containers)  
>    4.6 [Gameplay Abilities](#concepts-ga)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.6.1 [Gameplay Ability Definition](#concepts-ga-definition)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.6.1.1 [Replication Policy](#concepts-ga-definition-reppolicy)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.6.1.2 [Server Respects Remote Ability Cancellation](#concepts-ga-definition-remotecancel)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.6.1.3 [Replicate Input Directly](#concepts-ga-definition-repinputdirectly)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.6.2 [Binding Input to the ASC](#concepts-ga-input)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.6.2.1 [Binding to Input without Activating Abilities](#concepts-ga-input-noactivate)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.6.3 [Granting Abilities](#concepts-ga-granting)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.6.4 [Activating Abilities](#concepts-ga-activating)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.6.4.1 [Passive Abilities](#concepts-ga-activating-passive)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.6.4.2 [Activation Failed Tags](#concepts-ga-activating-failedtags)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.6.5 [Canceling Abilities](#concepts-ga-cancelabilities)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.6.6 [Getting Active Abilities](#concepts-ga-definition-activeability)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.6.7 [Instancing Policy](#concepts-ga-instancing)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.6.8 [Net Execution Policy](#concepts-ga-net)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.6.9 [Ability Tags](#concepts-ga-tags)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.6.10 [Gameplay Ability Spec](#concepts-ga-spec)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.6.11 [Passing Data to Abilities](#concepts-ga-data)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.6.12 [Ability Cost and Cooldown](#concepts-ga-commit)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.6.13 [Leveling Up Abilities](#concepts-ga-leveling)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.6.14 [Ability Sets](#concepts-ga-sets)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.6.15 [Ability Batching](#concepts-ga-batching)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.6.16 [Net Security Policy](#concepts-ga-netsecuritypolicy)  
>    4.7 [Ability Tasks](#concepts-at)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.7.1 [Ability Task Definition](#concepts-at-definition)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.7.2 [Custom Ability Tasks](#concepts-at-definition)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.7.3 [Using Ability Tasks](#concepts-at-using)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.7.4 [Root Motion Source Ability Tasks](#concepts-at-rms)  
>    4.8 [Gameplay Cues](#concepts-gc)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.8.1 [Gameplay Cue Definition](#concepts-gc-definition)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.8.2 [Triggering Gameplay Cues](#concepts-gc-trigger)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.8.3 [Local Gameplay Cues](#concepts-gc-local)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.8.4 [Gameplay Cue Parameters](#concepts-gc-parameters)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.8.5 [Gameplay Cue Manager](#concepts-gc-manager)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.8.6 [Prevent Gameplay Cues from Firing](#concepts-gc-prevention)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.8.7 [Gameplay Cue Batching](#concepts-gc-batching)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.8.7.1 [Manual RPC](#concepts-gc-batching-manualrpc)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.8.7.2 [Multiple GCs on one GE](#concepts-gc-batching-gcsonge)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.8.8 [Gameplay Cue Events](#concepts-gc-events)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.8.9 [Gameplay Cue Reliability](#concepts-gc-reliability)  
>    4.9 [Ability System Globals](#concepts-asg)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.9.1 [InitGlobalData()](#concepts-asg-initglobaldata)  
>    4.10 [Prediction](#concepts-p)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.10.1 [Prediction Key](#concepts-p-key)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.10.2 [Creating New Prediction Windows in Abilities](#concepts-p-windows)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.10.3 [Predictively Spawning Actors](#concepts-p-spawn)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.10.4 [Future of Prediction in GAS](#concepts-p-future)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.10.5 [Network Prediction Plugin](#concepts-p-npp)  
>    4.11 [Targeting](#concepts-targeting)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.11.1 [Target Data](#concepts-targeting-data)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.11.2 [Target Actors](#concepts-targeting-actors)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.11.3 [Target Data Filters](#concepts-target-data-filters)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.11.4 [Gameplay Ability World Reticles](#concepts-targeting-reticles)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.11.5 [Gameplay Effect Containers Targeting](#concepts-targeting-containers)  
> 1. [Commonly Implemented Abilities and Effects](#cae)  
>    5.1 [Stun](#cae-stun)  
>    5.2 [Sprint](#cae-sprint)  
>    5.3 [Aim Down Sights](#cae-ads)  
>    5.4 [Lifesteal](#cae-ls)  
>    5.5 [Generating a Random Number on Client and Server](#cae-random)  
>    5.6 [Critical Hits](#cae-crit)  
>    5.7 [Non-Stacking Gameplay Effects but Only the Greatest Magnitude Actually Affects the Target](#cae-nonstackingge)  
>    5.8 [Generate Target Data While Game is Paused](#cae-paused)  
>    5.9 [One Button Interaction System](#cae-onebuttoninteractionsystem)  
> 1. [Debugging GAS](#debugging)  
>    6.1 [showdebug abilitysystem](#debugging-sd)  
>    6.2 [Gameplay Debugger](#debugging-gd)  
>    6.3 [GAS Logging](#debugging-log)  
> 1. [Optimizations](#optimizations)  
>    7.1 [Ability Batching](#optimizations-abilitybatching)  
>    7.2 [Gameplay Cue Batching](#optimizations-gameplaycuebatching)  
>    7.3 [AbilitySystemComponent Replication Mode](#optimizations-ascreplicationmode)  
>    7.4 [Attribute Proxy Replication](#optimizations-attributeproxyreplication)  
>    7.5 [ASC Lazy Loading](#optimizations-asclazyloading)  
> 1. [Quality of Life Suggestions](#qol)  
>    8.1 [Gameplay Effect Containers](#qol-gameplayeffectcontainers)  
>    8.2 [Blueprint AsyncTasks to Bind to ASC Delegates](#qol-asynctasksascdelegates)  
> 1. [Troubleshooting](#troubleshooting)  
>    9.1 [`LogAbilitySystem: Warning: Can't activate LocalOnly or LocalPredicted ability %s when not local!`](#troubleshooting-notlocal)  
>    9.2 [`ScriptStructCache` errors](#troubleshooting-scriptstructcache)  
>    9.3 [Animation Montages are not replicating to clients](#troubleshooting-replicatinganimmontages)  
>    9.4 [Duplicating Blueprint Actors is setting AttributeSets to nullptr](#troubleshooting-duplicatingblueprintactors)  
>    9.5 [unresolved external symbol UEPushModelPrivate::MarkPropertyDirty(int,int)](#troubleshooting-unresolvedexternalsymbolmarkpropertydirty)  
>    9.6 [Enum names are now represented by path name](#troubleshooting-enumnamesarenowpathnames)  
> 1. [Common GAS Acronyms](#acronyms)
> 1. [Other Resources](#resources)  
>    11.1 [Q&A With Epic Game's Dave Ratti](#resources-daveratti)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;11.1.1 [Community Questions 1](#resources-daveratti-community1)  
>    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;11.1.2 [Community Questions 2](#resources-daveratti-community2)  
> 1. [GAS Changelog](#changelog)  
>    * [5.3](#changelog-5.3)  
>    * [5.2](#changelog-5.2)  
>    * [5.1](#changelog-5.1)  
>    * [5.0](#changelog-5.0)  
>    * [4.27](#changelog-4.27)  
>    * [4.26](#changelog-4.26)  
>    * [4.25.1](#changelog-4.25.1)  
>    * [4.25](#changelog-4.25)  
>    * [4.24](#changelog-4.24)
         
<a name="intro"></a>
## 1. Intro to the GameplayAbilitySystem Plugin
From the [Official Documentation](https://docs.unrealengine.com/en-US/Gameplay/GameplayAbilitySystem/index.html):
>게임플레이 어빌리티 시스템은 RPG나 MOBA 게임에서 볼 수 있는 유형의 능력과 속성을 구축하기 위한 매우 유연한 프레임워크입니다. 이 시스템을 통해 게임 캐릭터가 사용할 수 있는 행동이나 패시브 능력을 만들고, 이러한 행동의 결과로 다양한 속성을 증가시키거나 감소시키는 상태 효과를 구현하며, 이러한 행동의 사용을 조절하기 위한 '쿨다운' 타이머나 자원 소모를 구현할 수 있습니다. 또한 능력의 레벨과 각 레벨에서의 효과를 변경하고, 파티클이나 사운드 효과를 활성화하는 등의 기능을 구현할 수 있습니다. 간단히 말해, 이 시스템은 점프와 같은 단순한 능력부터 현대의 RPG나 MOBA 게임에서 볼 수 있는 가장 좋아하는 캐릭터의 복잡한 능력 세트에 이르기까지, 게임 내 능력을 설계하고 구현하며 효율적으로 네트워크화하는 데 도움을 줄 수 있습니다.

GameplayAbilitySystem 플러그인은 Epic Games에서 개발했으며 언리얼 엔진에 포함되어 있습니다. 이 플러그인은 Paragon과 Fortnite를 비롯한 여러 AAA 상업 게임에서 실전 검증을 거쳤습니다

이 플러그인은 싱글플레이어 및 멀티플레이어 게임에서 다음과 같은 기능을 즉시 사용 가능한 솔루션으로 제공합니다:
* 선택적 비용과 쿨다운이 있는 레벨 기반 캐릭터 능력이나 스킬 구현 ([GameplayAbilities](#concepts-ga))
* 액터에 속한 수치형 `Attributes` 조작 ([Attributes](#concepts-a))
* 액터에 상태 효과 적용 ([GameplayEffects](#concepts-ge))
* 액터에 `GameplayTags` 적용 ([GameplayTags](#concepts-gt))
* 시각 효과나 사운드 효과 생성 ([GameplayCues](#concepts-gc))
* 위에 언급된 모든 것의 리플리케이션

멀티플레이어 게임에서 GAS는 다음에 대한 [client-side prediction](#concepts-p) 예측을 지원합니다:

* 어빌리티 활성화
* 애니메이션 몽타주 재생
* `Attributes` 변경
* `GameplayTags` 적용
* `GameplayCues` 생성
* `CharacterMovementComponent`에 연결된 `RootMotionSource` 함수를 통한 이동

**GAS는 반드시 C++에서 설정해야 하지만**, `GameplayAbilities`와 `GameplayEffects`는 디자이너가 블루프린트에서 생성할 수 있습니다.

GAS의 현재 문제점:
* `GameplayEffect` 지연 시간 조정(어빌리티 쿨다운을 예측할 수 없어, 지연 시간이 높은 플레이어들이 지연 시간이 낮은 플레이어들에 비해 낮은 쿨다운 어빌리티의 발사 속도가 더 느림).
* `GameplayEffects`의 제거를 예측할 수 없음. 하지만 역효과를 가진 `GameplayEffects`를 추가하여 예측함으로써 효과적으로 제거할 수 있습니다. 이는 항상 적절하거나 실현 가능한 것은 아니며 여전히 문제로 남아 있습니다.
* 상용구 템플릿, 멀티플레이어 예제, 문서의 부족. 이 문서가 그 부분에 도움이 되기를 바랍니다!

**[⬆ Back to Top](#table-of-contents)**

<a name="sp"></a>
## 2. 샘플 프로젝트
이 문서에는 GameplayAbilitySystem 플러그인을 처음 접하지만 언리얼 엔진에는 익숙한 사람들을 위한 멀티플레이어 3인칭 슈터 샘플 프로젝트가 포함되어 있습니다. 사용자들은 C++, 블루프린트, UMG, 리플리케이션 및 UE의 기타 중급 주제에 대해 알고 있어야 합니다. 이 프로젝트는 플레이어/AI가 제어하는 영웅의 경우 `PlayerState` 클래스에 `AbilitySystemComponent`(`ASC`)를 두고, AI가 제어하는 미니언의 경우 `Character` 클래스에 `ASC`를 둔 기본적인 3인칭 슈터 멀티플레이어 준비 프로젝트를 설정하는 방법의 예를 제공합니다.

이 프로젝트의 목표는 GAS 기본 사항을 보여주고 잘 주석 처리된 코드로 일반적으로 요청되는 몇 가지 능력을 시연하면서 단순함을 유지하는 것입니다. 초보자에 초점을 맞추고 있기 때문에, 이 프로젝트는 [predicting projectiles](#concepts-p-spawn)과 같은 고급 주제는 다루지 않습니다. 

시연되는 개념:
* `PlayerState` vs `Character`에서의 `ASC`
* 복제된 `Attributes`
* 복제된 애니메이션 몽타주
* `GameplayTags`
* `GameplayAbilities` 내부 및 외부에서 `GameplayEffects` 적용 및 제거
* 방어구로 완화된 데미지를 캐릭터의 체력 변경에 적용
* `GameplayEffectExecutionCalculations`
* 스턴 효과
* 사망 및 리스폰
* 서버에서 능력을 통한 액터(발사체) 생성
* 조준 사격 및 질주 시 로컬 플레이어의 속도를 예측적으로 변경
* 질주를 위해 지속적으로 스태미나 소모
* 능력 사용을 위한 마나 사용
* 패시브 능력
* `GameplayEffects` 스택킹
* 액터 타겟팅
* 블루프린트에서 생성된 `GameplayAbilities`
* C++에서 생성된 `GameplayAbilities`
* `액터`별로 인스턴스화된 `GameplayAbilities`
* 인스턴스화되지 않은 `GameplayAbilities` (점프)
* 정적 `GameplayCues` (발사체 충돌 파티클 효과)
* 액터 `GameplayCues` (질주 및 스턴 파티클 효과)

히어로 클래스는 다음과 같은 능력을 가지고 있습니다:
| 능력                       | 입력 바인딩         | 예측 여부  | C++ / 블루프린트 | 설명                                                                                                                                                                        |
| -------------------------- | ------------------- | ---------- | ---------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 점프                       | 스페이스바          | 예         | C++              | 히어로를 점프하게 합니다.                                                                                                                                                    |
| 총                         | 왼쪽 마우스 버튼    | 아니오     | C++              | 히어로의 총에서 발사체를 발사합니다. 애니메이션은 예측되지만 발사체는 예측되지 않습니다.                                                                                    |
| 조준 사격                  | 오른쪽 마우스 버튼  | 예         | 블루프린트       | 버튼을 누르고 있는 동안 히어로의 걷는 속도가 느려지고 카메라가 확대되어 총으로 더 정확한 사격을 할 수 있게 합니다.                                                         |
| 질주                       | 왼쪽 Shift          | 예         | 블루프린트       | 버튼을 누르고 있는 동안 히어로가 스태미나를 소모하며 더 빠르게 달립니다.                                                                                                    |
| 전방 대시                  | Q                   | 예         | 블루프린트       | 히어로가 스태미나를 소모하며 전방으로 대시합니다.                                                                                                                            |
| 패시브 방어구 스택         | 패시브              | 아니오     | 블루프린트       | 4초마다 히어로가 방어구 스택을 얻으며 최대 4 스택까지 쌓을 수 있습니다. 데미지를 받으면 방어구 스택이 하나 제거됩니다.                                                     |
| 메테오                     | R                   | 아니오     | 블루프린트       | 플레이어가 적에게 데미지를 주고 기절시키는 메테오를 떨어뜨릴 위치를 지정합니다. 타겟팅은 예측되지만 메테오 생성은 예측되지 않습니다.                                       |

`GameplayAbilities`가 C++에서 생성되었는지 블루프린트에서 생성되었는지는 중요하지 않습니다. 여기서는 각 언어에서 어떻게 구현하는지 예시를 위해 둘을 혼합하여 사용했습니다.

미니언은 사전 정의된 `GameplayAbilities`를 가지고 있지 않습니다. 레드 미니언은 체력 재생이 더 많고 블루 미니언은 시작 체력이 더 높습니다.

`GameplayAbility` 명명에 있어서, 블루프린트에서 생성된 `GameplayAbility`의 로직을 나타내기 위해 접미사 `_BP`를 사용했습니다. 접미사가 없으면 C++에서 로직이 생성되었음을 의미합니다.

**블루프린트 에셋 명명 접두사**
| 접두사      | 에셋 유형           |
| ----------- | ------------------- |
| GA_         | GameplayAbility     |
| GC_         | GameplayCue         |
| GE_         | GameplayEffect      |

**[⬆ Back to Top](#table-of-contents)**

<a name="setup"></a>
## 3. GAS를 사용하여 프로젝트 설정하기
GAS를 사용하여 프로젝트를 설정하는 기본 단계:
1. 에디터에서 GameplayAbilitySystem 플러그인 활성화
2. `YourProjectName.Build.cs`를 편집하여 `PrivateDependencyModuleNames`에 `"GameplayAbilities", "GameplayTags", "GameplayTasks"`를 추가
3. Visual Studio 프로젝트 파일 새로 고침/재생성
4. 4.24부터 5.2 버전까지는 [`TargetData`](#concepts-targeting-data)를 사용하기 위해 `UAbilitySystemGlobals::Get().InitGlobalData()`를 호출하는 것이 필수입니다. 샘플 프로젝트는 `UAssetManager::StartInitialLoading()`에서 이를 수행합니다. 5.3 버전부터는 자동으로 호출됩니다. 자세한 정보는 [`InitGlobalData()`](#concepts-asg-initglobaldata)를 참조하세요.

GAS를 활성화하기 위해 해야 할 일은 이것이 전부입니다. 여기서부터 [`ASC`](#concepts-asc)와 [`AttributeSet`](#concepts-as)을 `Character`나 `PlayerState`에 추가하고 [`GameplayAbilities`](#concepts-ga)와 [`GameplayEffects`](#concepts-ge)를 만들기 시작하세요!

**[⬆ Back to Top](#table-of-contents)**

<a name="concepts"></a>
## 4. GAS 개념

#### 섹션

> 4.1 [어빌리티 시스템 컴포넌트](#concepts-asc)  
> 4.2 [게임플레이 태그](#concepts-gt)  
> 4.3 [어트리뷰트](#concepts-a)  
> 4.4 [어트리뷰트 세트](#concepts-as)  
> 4.5 [게임플레이 이펙트](#concepts-ge)  
> 4.6 [게임플레이 어빌리티](#concepts-ga)  
> 4.7 [어빌리티 태스크](#concepts-at)  
> 4.8 [게임플레이 큐](#concepts-gc)  
> 4.9 [어빌리티 시스템 글로벌](#concepts-asg)  
> 4.10 [예측](#concepts-p)

<a name="concepts-asc"></a>
### 4.1 어빌리티 시스템 컴포넌트
`AbilitySystemComponent`(`ASC`)는 GAS의 핵심입니다. 이는 시스템과의 모든 상호작용을 처리하는 `UActorComponent`([`UAbilitySystemComponent`](https://docs.unrealengine.com/en-US/API/Plugins/GameplayAbilities/UAbilitySystemComponent/index.html))입니다. [`GameplayAbilities`](#concepts-ga)를 사용하거나, [`Attributes`](#concepts-a)를 가지거나, [`GameplayEffects`](#concepts-ge)를 받고자 하는 모든 `Actor`는 하나의 `ASC`가 부착되어 있어야 합니다. 이러한 객체들은 모두 `ASC` 내부에 존재하며, `ASC`에 의해 관리되고 복제됩니다(단, `Attributes`는 [`AttributeSet`](#concepts-as)에 의해 복제됩니다). 개발자들은 이를 서브클래싱할 것으로 예상되지만 필수는 아닙니다.

`ASC`가 부착된 `Actor`는 `ASC`의 `OwnerActor`라고 불립니다. `ASC`의 물리적 표현 `Actor`는 `AvatarActor`라고 불립니다. `OwnerActor`와 `AvatarActor`는 MOBA 게임의 단순한 AI 미니언의 경우처럼 동일한 `Actor`일 수 있습니다. 또한 MOBA 게임의 플레이어가 제어하는 영웅의 경우처럼 다른 `Actor`일 수도 있는데, 이 경우 `OwnerActor`는 `PlayerState`이고 `AvatarActor`는 영웅의 `Character` 클래스입니다. 대부분의 `Actor`는 자신에게 `ASC`를 가지게 될 것입니다. 만약 당신의 `Actor`가 리스폰되고 스폰 사이에 `Attributes`나 `GameplayEffects`의 지속성이 필요하다면(MOBA의 영웅처럼), `ASC`의 이상적인 위치는 `PlayerState`입니다.

**참고:** 만약 당신의 `ASC`가 `PlayerState`에 있다면, `PlayerState`의 `NetUpdateFrequency`를 증가시켜야 할 것입니다. `PlayerState`의 기본값은 매우 낮아서 `Attributes`와 `GameplayTags` 같은 것들의 변경이 클라이언트에 반영되기 전에 지연이나 인지된 지연이 발생할 수 있습니다. [`Adaptive Network Update Frequency`](https://docs.unrealengine.com/en-US/Gameplay/Networking/Actors/Properties/index.html#adaptivenetworkupdatefrequency)를 활성화하세요. Fortnite도 이를 사용합니다.

`OwnerActor`와 `AvatarActor`가 다른 `Actor`인 경우, 둘 다 `IAbilitySystemInterface`를 구현해야 합니다. 이 인터페이스에는 반드시 오버라이드해야 하는 함수가 하나 있습니다: `UAbilitySystemComponent* GetAbilitySystemComponent() const`. 이 함수는 자신의 `ASC`에 대한 포인터를 반환합니다. `ASC`들은 시스템 내부적으로 이 인터페이스 함수를 찾아 서로 상호작용합니다.

`ASC`는 현재 활성화된 `GameplayEffects`를 `FActiveGameplayEffectsContainer ActiveGameplayEffects`에 보관합니다.

`ASC`는 부여된 `Gameplay Abilities`를 `FGameplayAbilitySpecContainer ActivatableAbilities`에 보관합니다. `ActivatableAbilities.Items`를 순회할 계획이 있다면, 반드시 루프 위에 `ABILITYLIST_SCOPE_LOCK();`을 추가하여 리스트가 변경되지 않도록 잠가야 합니다(어빌리티 제거로 인한 변경 방지). 스코프 내의 모든 `ABILITYLIST_SCOPE_LOCK();`는 `AbilityScopeLockCount`를 증가시키고 스코프를 벗어나면 감소시킵니다. `ABILITYLIST_SCOPE_LOCK();`의 스코프 내에서 어빌리티를 제거하려고 하지 마세요(clear ability 함수들은 내부적으로 `AbilityScopeLockCount`를 확인하여 리스트가 잠겨있으면 어빌리티 제거를 방지합니다).

<a name="concepts-asc-rm"></a>
### 4.1.1 복제 모드
`ASC`는 `GameplayEffects`, `GameplayTags`, 그리고 `GameplayCues`를 복제하기 위한 세 가지 다른 복제 모드를 정의합니다 - `Full`, `Mixed`, 그리고 `Minimal`. `Attributes`는 그들의 `AttributeSet`에 의해 복제됩니다.

| 복제 모드          | 사용 시기                              | 설명                                                                                                                           |
| ------------------ | -------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| `Full`             | 싱글 플레이어                          | 모든 `GameplayEffect`가 모든 클라이언트에 복제됩니다.                                                                          |
| `Mixed`            | 멀티플레이어, 플레이어 제어 `Actors`   | `GameplayEffects`는 소유 클라이언트에만 복제됩니다. `GameplayTags`와 `GameplayCues`만 모두에게 복제됩니다.                    |
| `Minimal`          | 멀티플레이어, AI 제어 `Actors`         | `GameplayEffects`는 누구에게도 복제되지 않습니다. `GameplayTags`와 `GameplayCues`만 모두에게 복제됩니다.                      |

**참고:** `Mixed` 복제 모드는 `OwnerActor`의 `Owner`가 `Controller`일 것으로 예상합니다. `PlayerState`의 `Owner`는 기본적으로 `Controller`이지만 `Character`의 경우는 그렇지 않습니다. `OwnerActor`가 `PlayerState`가 아닌 상태에서 `Mixed` 복제 모드를 사용한다면, 유효한 `Controller`로 `OwnerActor`에 대해 `SetOwner()`를 호출해야 합니다.

4.24 버전부터, `PossessedBy()`는 이제 `Pawn`의 소유자를 새로운 `Controller`로 설정합니다.

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-asc-setup"></a>
### 4.1.2 설정 및 초기화
`ASC`는 일반적으로 `OwnerActor`의 생성자에서 구성되며 명시적으로 복제되도록 표시됩니다. **이는 반드시 C++에서 수행해야 합니다**.

```c++
AGDPlayerState::AGDPlayerState()
{
	// 어빌리티 시스템 컴포넌트를 생성하고 명시적으로 복제되도록 설정
	AbilitySystemComponent = CreateDefaultSubobject<UGDAbilitySystemComponent>(TEXT("AbilitySystemComponent"));
	AbilitySystemComponent->SetIsReplicated(true);
	//...
}
```

`ASC`는 서버와 클라이언트 모두에서 `OwnerActor`와 `AvatarActor`로 초기화되어야 합니다. `Pawn`의 `Controller`가 설정된 후(소유 후)에 초기화하고자 합니다. 싱글 플레이어 게임은 서버 경로만 고려하면 됩니다.

`ASC`가 `Pawn`에 있는 플레이어 제어 캐릭터의 경우, 일반적으로 서버에서는 `Pawn`의 `PossessedBy()` 함수에서 초기화하고 클라이언트에서는 `PlayerController`의 `AcknowledgePossession()` 함수에서 초기화합니다.

```c++
void APACharacterBase::PossessedBy(AController * NewController)
{
	Super::PossessedBy(NewController);
	if (AbilitySystemComponent)
	{
		AbilitySystemComponent->InitAbilityActorInfo(this, this);
	}
	// ASC MixedMode 복제는 ASC 소유자의 소유자가 Controller여야 합니다.
	SetOwner(NewController);
}
```

```c++
void APAPlayerControllerBase::AcknowledgePossession(APawn* P)
{
	Super::AcknowledgePossession(P);
	APACharacterBase* CharacterBase = Cast<APACharacterBase>(P);
	if (CharacterBase)
	{
		CharacterBase->GetAbilitySystemComponent()->InitAbilityActorInfo(CharacterBase, CharacterBase);
	}
	//...
}
```

`ASC`가 `PlayerState`에 있는 플레이어 제어 캐릭터의 경우, 일반적으로 서버에서는 `Pawn`의 `PossessedBy()` 함수에서 초기화하고 클라이언트에서는 `Pawn`의 `OnRep_PlayerState()` 함수에서 초기화합니다. 이는 클라이언트에서 `PlayerState`가 존재함을 보장합니다.

```c++
// 서버에서만
void AGDHeroCharacter::PossessedBy(AController * NewController)
{
	Super::PossessedBy(NewController);
	AGDPlayerState* PS = GetPlayerState<AGDPlayerState>();
	if (PS)
	{
		// 서버에서 ASC를 설정합니다. 클라이언트는 OnRep_PlayerState()에서 이를 수행합니다.
		AbilitySystemComponent = Cast<UGDAbilitySystemComponent>(PS->GetAbilitySystemComponent());
		// AI는 PlayerController를 가지지 않으므로 여기서 다시 초기화할 수 있습니다. PlayerController를 가진 영웅의 경우 두 번 초기화해도 해롭지 않습니다.
		PS->GetAbilitySystemComponent()->InitAbilityActorInfo(PS, this);
	}
	
	//...
}
```

```c++
// 클라이언트에서만
void AGDHeroCharacter::OnRep_PlayerState()
{
	Super::OnRep_PlayerState();
	AGDPlayerState* PS = GetPlayerState<AGDPlayerState>();
	if (PS)
	{
		// 클라이언트를 위해 ASC를 설정합니다. 서버는 PossessedBy에서 이를 수행합니다.
		AbilitySystemComponent = Cast<UGDAbilitySystemComponent>(PS->GetAbilitySystemComponent());
		// 클라이언트를 위해 ASC Actor 정보를 초기화합니다. 서버는 새로운 Actor를 소유할 때 ASC를 초기화합니다.
		AbilitySystemComponent->InitAbilityActorInfo(PS, this);
	}
	// ...
}
```

`LogAbilitySystem: Warning: Can't activate LocalOnly or LocalPredicted ability %s when not local!` 오류 메시지가 나타나면 클라이언트에서 `ASC`를 초기화하지 않은 것입니다.
**[⬆ Back to Top](#table-of-contents)**

<a name="concepts-gt"></a>
### 4.2 게임플레이 태그

[`FGameplayTags`](https://docs.unrealengine.com/en-US/API/Runtime/GameplayTags/FGameplayTag/index.html)는 `Parent.Child.Grandchild...` 형태의 계층적 이름으로, `GameplayTagManager`에 등록됩니다. 이 태그들은 객체의 상태를 분류하고 설명하는 데 매우 유용합니다. 예를 들어, 캐릭터가 기절했다면 기절 지속 시간 동안 `State.Debuff.Stun` `GameplayTag`를 부여할 수 있습니다.

불리언이나 열거형으로 처리하던 것들을 `GameplayTags`로 대체하고, 객체가 특정 `GameplayTags`를 가지고 있는지에 대한 불리언 로직을 수행하게 될 것입니다.

객체에 태그를 부여할 때, 일반적으로 GAS가 이와 상호작용할 수 있도록 ASC가 있는 경우 ASC에 추가합니다. `UAbilitySystemComponent`는 `IGameplayTagAssetInterface`를 구현하여 소유한 `GameplayTags`에 접근하는 함수를 제공합니다.

여러 `GameplayTags`는 `FGameplayTagContainer`에 저장될 수 있습니다. `GameplayTagContainers`가 일부 효율성 매직을 추가하기 때문에 `TArray<FGameplayTag>` 대신 `GameplayTagContainer`를 사용하는 것이 좋습니다. 태그는 표준 `FNames`이지만, 프로젝트 설정에서 `Fast Replication`이 활성화된 경우 `FGameplayTagContainers`에서 복제를 위해 효율적으로 압축될 수 있습니다. `Fast Replication`은 서버와 클라이언트가 동일한 `GameplayTags` 목록을 가지고 있어야 합니다. 일반적으로 이는 문제가 되지 않으므로 이 옵션을 활성화해야 합니다. `GameplayTagContainers`는 반복을 위해 `TArray<FGameplayTag>`를 반환할 수도 있습니다.

`FGameplayTagCountContainer`에 저장된 `GameplayTags`는 해당 `GameplayTag`의 인스턴스 수를 저장하는 `TagMap`을 가집니다. `FGameplayTagCountContainer`는 여전히 `GameplayTag`를 가지고 있을 수 있지만 `TagMapCount`가 0일 수 있습니다. ASC가 여전히 `GameplayTag`를 가지고 있는지 디버깅하는 동안 이를 발견할 수 있습니다. `HasTag()` 또는 `HasMatchingTag()` 또는 유사한 함수는 `TagMapCount`를 확인하고 `GameplayTag`가 없거나 `TagMapCount`가 0이면 false를 반환합니다.

`GameplayTags`는 `DefaultGameplayTags.ini`에 미리 정의되어야 합니다. 언리얼 엔진 에디터는 개발자가 `DefaultGameplayTags.ini`를 수동으로 편집할 필요 없이 `GameplayTags`를 관리할 수 있는 프로젝트 설정 인터페이스를 제공합니다. `GameplayTag` 에디터는 `GameplayTags`를 생성, 이름 변경, 참조 검색, 삭제할 수 있습니다.

![프로젝트 설정의 GameplayTag 에디터](https://github.com/tranek/GASDocumentation/raw/master/Images/gameplaytageditor.png)

`GameplayTag` 참조 검색은 익숙한 `Reference Viewer` 그래프를 에디터에 표시하여 `GameplayTag`를 참조하는 모든 에셋을 보여줍니다. 그러나 이는 `GameplayTag`를 참조하는 C++ 클래스는 보여주지 않습니다.

`GameplayTags`의 이름을 변경하면 리디렉션이 생성되어 원래 `GameplayTag`를 여전히 참조하는 에셋이 새로운 `GameplayTag`로 리디렉션될 수 있습니다. 가능하다면 리디렉션 생성을 피하기 위해 새로운 `GameplayTag`를 생성하고, 모든 참조를 새로운 `GameplayTag`로 수동으로 업데이트한 다음 이전 `GameplayTag`를 삭제하는 것을 선호합니다.

`Fast Replication` 외에도 `GameplayTag` 에디터에는 일반적으로 복제되는 `GameplayTags`를 채워 더욱 최적화하는 옵션이 있습니다.

`GameplayTags`는 `GameplayEffect`에서 추가된 경우 복제됩니다. ASC는 복제되지 않고 수동으로 관리해야 하는 `LooseGameplayTags`를 추가할 수 있게 합니다. 샘플 프로젝트는 `State.Dead`에 대해 `LooseGameplayTag`를 사용하여 소유 클라이언트가 체력이 0으로 떨어질 때 즉시 반응할 수 있도록 합니다. 리스폰은 수동으로 `TagMapCount`를 0으로 설정합니다. `LooseGameplayTags`로 작업할 때만 `TagMapCount`를 수동으로 조정하세요. `TagMapCount`를 수동으로 조정하는 것보다 `UAbilitySystemComponent::AddLooseGameplayTag()` 및 `UAbilitySystemComponent::RemoveLooseGameplayTag()` 함수를 사용하는 것이 좋습니다.

C++에서 `GameplayTag` 참조 가져오기:
```c++
FGameplayTag::RequestGameplayTag(FName("Your.GameplayTag.Name"))
```

부모나 자식 `GameplayTags` 가져오기와 같은 고급 `GameplayTag` 조작을 위해서는 `GameplayTagManager`가 제공하는 함수를 살펴보세요. `GameplayTagManager`에 접근하려면 `GameplayTagManager.h`를 포함하고 `UGameplayTagManager::Get().FunctionName`으로 호출하세요. `GameplayTagManager`는 실제로 `GameplayTags`를 관계형 노드(부모, 자식 등)로 저장하여 지속적인 문자열 조작 및 비교보다 더 빠른 처리를 가능하게 합니다.

`GameplayTags`와 `GameplayTagContainers`는 선택적으로 `UPROPERTY` 지정자 `Meta = (Categories = "GameplayCue")`를 가질 수 있으며, 이는 블루프린트에서 태그를 필터링하여 `GameplayCue` 부모 태그를 가진 `GameplayTags`만 표시합니다. 이는 `GameplayTag` 또는 `GameplayTagContainer` 변수가 `GameplayCues`에만 사용되어야 한다는 것을 알고 있을 때 유용합니다.

또는 `FGameplayCueTag`라는 별도의 구조체가 있어 `FGameplayTag`를 캡슐화하고 블루프린트에서 `GameplayTags`를 자동으로 필터링하여 `GameplayCue` 부모 태그를 가진 태그만 표시합니다.

함수의 `GameplayTag` 매개변수를 필터링하려면 `UFUNCTION` 지정자 `Meta = (GameplayTagFilter = "GameplayCue")`를 사용하세요. 함수의 `GameplayTagContainer` 매개변수는 필터링할 수 없습니다. 엔진을 편집하여 이를 허용하고 싶다면, `Engine\Plugins\Editor\GameplayTagsEditor\Source\GameplayTagsEditor\Private\SGameplayTagGraphPin.cpp`의 `SGameplayTagGraphPin::ParseDefaultValueData()`가 `FilterString = UGameplayTagsManager::Get().GetCategoriesMetaFromField(PinStructType);`를 호출하고 `SGameplayTagGraphPin::GetListContent()`에서 `SGameplayTagWidget`에 `FilterString`을 전달하는 방법을 살펴보세요. `Engine\Plugins\Editor\GameplayTagsEditor\Source\GameplayTagsEditor\Private\SGameplayTagContainerGraphPin.cpp`의 `GameplayTagContainer` 버전 함수는 메타 필드 속성을 확인하지 않고 필터를 전달하지 않습니다.

샘플 프로젝트는 `GameplayTags`를 광범위하게 사용합니다.

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-gt-change"></a>
### 4.2.1 게임플레이 태그 변경에 대한 응답
ASC는 `GameplayTags`가 추가되거나 제거될 때 델리게이트를 제공합니다. 이는 `GameplayTag`가 추가/제거될 때만 실행되거나 `GameplayTag`의 `TagMapCount`의 모든 변경에 대해 실행되도록 지정할 수 있는 `EGameplayTagEventType`을 받습니다.

```c++
AbilitySystemComponent->RegisterGameplayTagEvent(FGameplayTag::RequestGameplayTag(FName("State.Debuff.Stun")), EGameplayTagEventType::NewOrRemoved).AddUObject(this, &AGDPlayerState::StunTagChanged);
```

콜백 함수는 `GameplayTag`와 새로운 `TagCount`에 대한 매개변수를 가집니다.
```c++
virtual void StunTagChanged(const FGameplayTag CallbackTag, int32 NewCount);
```

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-gt-loadfromplugin"></a>
### 4.2.2 플러그인 .ini 파일에서 게임플레이 태그 로딩
`GameplayTags`가 있는 자체 .ini 파일을 가진 플러그인을 만든 경우, 플러그인의 `StartupModule()` 함수에서 해당 플러그인의 `GameplayTag` .ini 디렉토리를 로드할 수 있습니다.

예를 들어, 언리얼 엔진과 함께 제공되는 CommonConversation 플러그인은 다음과 같이 수행합니다:

```c++
void FCommonConversationRuntimeModule::StartupModule()
{
	TSharedPtr<IPlugin> ThisPlugin = IPluginManager::Get().FindPlugin(TEXT("CommonConversation"));
	check(ThisPlugin.IsValid());
	
	UGameplayTagsManager::Get().AddTagIniSearchPath(ThisPlugin->GetBaseDir() / TEXT("Config") / TEXT("Tags"));

	//...
}
```

이는 `Plugins\CommonConversation\Config\Tags` 디렉토리를 찾아 플러그인이 활성화된 경우 엔진이 시작될 때 해당 디렉토리의 `GameplayTags`가 포함된 모든 .ini 파일을 프로젝트에 로드합니다.

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-a"></a>
### 4.3 어트리뷰트

<a name="concepts-a-definition"></a>
#### 4.3.1 어트리뷰트 정의
`Attributes`는 [`FGameplayAttributeData`](https://docs.unrealengine.com/en-US/API/Plugins/GameplayAbilities/FGameplayAttributeData/index.html) 구조체로 정의된 float 값입니다. 이는 캐릭터의 체력량, 캐릭터의 레벨, 포션의 충전 횟수 등 다양한 것을 나타낼 수 있습니다. `Actor`에 속하는 게임플레이 관련 수치 값이라면 `Attribute`를 사용하는 것을 고려해야 합니다. `Attributes`는 일반적으로 ASC가 변경을 [예측](#concepts-p)할 수 있도록 [`GameplayEffects`](#concepts-ge)에 의해서만 수정되어야 합니다.

`Attributes`는 [`AttributeSet`](#concepts-as)에 의해 정의되고 그 안에 존재합니다. `AttributeSet`는 복제를 위해 표시된 `Attributes`를 복제하는 책임을 집니다. `Attributes` 정의 방법에 대해서는 [`AttributeSets`](#concepts-as) 섹션을 참조하세요.

**팁:** 에디터의 `Attributes` 목록에 `Attribute`가 표시되지 않게 하려면 `Meta = (HideInDetailsView)` `property specifier`를 사용할 수 있습니다.

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-a-value"></a>
#### 4.3.2 BaseValue vs CurrentValue
`Attribute`는 `BaseValue`와 `CurrentValue` 두 가지 값으로 구성됩니다. `BaseValue`는 `Attribute`의 영구적인 값이며, `CurrentValue`는 `BaseValue`에 `GameplayEffects`로부터의 임시 수정사항을 더한 값입니다. 예를 들어, 당신의 `Character`가 600 단위/초의 `BaseValue`를 가진 이동속도 `Attribute`를 가지고 있다고 가정해봅시다. 아직 이동속도를 수정하는 `GameplayEffects`가 없으므로 `CurrentValue`도 600 u/s입니다. 만약 임시로 50 u/s의 이동속도 버프를 받으면, `BaseValue`는 여전히 600 u/s로 유지되지만 `CurrentValue`는 600 + 50으로 총 650 u/s가 됩니다. 이동속도 버프가 만료되면 `CurrentValue`는 다시 `BaseValue`인 600 u/s로 돌아갑니다.

GAS 초보자들은 종종 `BaseValue`를 `Attribute`의 최대값으로 혼동하고 그렇게 취급하려고 합니다. 이는 잘못된 접근 방식입니다. 변경될 수 있거나 능력이나 UI에서 참조되는 `Attributes`의 최대값은 별도의 `Attributes`로 취급해야 합니다. 하드코딩된 최대값과 최소값의 경우, `FAttributeMetaData`로 `DataTable`을 정의하여 최대값과 최소값을 설정할 수 있는 방법이 있지만, Epic의 구조체 위의 주석은 이를 "진행 중인 작업"이라고 부릅니다. 자세한 정보는 `AttributeSet.h`를 참조하세요. 혼란을 방지하기 위해, 능력이나 UI에서 참조될 수 있는 최대값은 별도의 `Attributes`로 만들고, `Attributes`를 제한하는 데만 사용되는 하드코딩된 최대값과 최소값은 `AttributeSet`에서 하드코딩된 floats로 정의하는 것을 권장합니다. `Attributes`의 제한은 `CurrentValue` 변경에 대해서는 [PreAttributeChange()](#concepts-as-preattributechange)에서, `GameplayEffects`로부터의 `BaseValue` 변경에 대해서는 [PostGameplayEffectExecute()](#concepts-as-postgameplayeffectexecute)에서 논의됩니다.

`BaseValue`의 영구적인 변경은 `Instant` `GameplayEffects`에서 오는 반면, `Duration` 및 `Infinite` `GameplayEffects`는 `CurrentValue`를 변경합니다. 주기적 `GameplayEffects`는 즉시 `GameplayEffects`처럼 취급되어 `BaseValue`를 변경합니다.

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-a-meta"></a>
#### 4.3.3 메타 어트리뷰트
일부 `Attributes`는 `Attributes`와 상호작용하기 위한 임시 값의 자리 표시자로 취급됩니다. 이를 `Meta Attributes`라고 합니다. 예를 들어, 우리는 일반적으로 데미지를 `Meta Attribute`로 정의합니다. `GameplayEffect`가 우리의 체력 `Attribute`를 직접 변경하는 대신, 우리는 데미지라는 `Meta Attribute`를 자리 표시자로 사용합니다. 이렇게 하면 데미지 값이 [`GameplayEffectExecutionCalculation`](#concepts-ge-ec)에서 버프와 디버프로 수정될 수 있고, `AttributeSet`에서 더 조작될 수 있습니다. 예를 들어, 현재 방어막 `Attribute`에서 데미지를 빼고, 마지막으로 나머지를 체력 `Attribute`에서 빼는 식입니다. 데미지 `Meta Attribute`는 `GameplayEffects` 사이에 지속성이 없으며 매번 덮어씁니다. `Meta Attributes`는 일반적으로 복제되지 않습니다.

`Meta Attributes`는 데미지와 치유와 같은 것들에 대해 "얼마나 많은 데미지를 주었는가?"와 "이 데미지로 무엇을 할 것인가?" 사이의 좋은 논리적 분리를 제공합니다. 이러한 논리적 분리는 우리의 `Gameplay Effects`와 `Execution Calculations`이 대상이 데미지를 어떻게 처리하는지 알 필요가 없다는 것을 의미합니다. 데미지 예를 계속하면, `Gameplay Effect`는 얼마나 많은 데미지를 주는지 결정하고, 그 다음 `AttributeSet`가 그 데미지로 무엇을 할지 결정합니다. 모든 캐릭터가 동일한 `Attributes`를 가지고 있지 않을 수 있습니다. 특히 서브클래스된 `AttributeSets`을 사용하는 경우에 그렇습니다. 기본 `AttributeSet` 클래스는 체력 `Attribute`만 가지고 있을 수 있지만, 서브클래스된 `AttributeSet`는 방어막 `Attribute`를 추가할 수 있습니다. 방어막 `Attribute`가 있는 서브클래스된 `AttributeSet`는 받은 데미지를 기본 `AttributeSet` 클래스와는 다르게 분배할 것입니다.

`Meta Attributes`는 좋은 설계 패턴이지만, 필수는 아닙니다. 모든 데미지 인스턴스에 대해 하나의 `Execution Calculation`만 사용하고 모든 캐릭터가 공유하는 하나의 `Attribute Set` 클래스만 있다면, `Execution Calculation` 내에서 체력, 방어막 등에 대한 데미지 분배를 수행하고 해당 `Attributes`를 직접 수정하는 것도 괜찮을 수 있습니다. 단지 유연성을 희생하게 되지만, 그것이 당신에게는 괜찮을 수 있습니다.

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-a-changes"></a>
#### 4.3.4 어트리뷰트 변경에 대한 응답
UI나 다른 게임플레이를 업데이트하기 위해 `Attribute`가 변경될 때 듣기 위해서는 `UAbilitySystemComponent::GetGameplayAttributeValueChangeDelegate(FGameplayAttribute Attribute)`를 사용하세요. 이 함수는 `Attribute`가 변경될 때마다 자동으로 호출되는 델리게이트를 반환합니다. 델리게이트는 `NewValue`, `OldValue`, 그리고 `FGameplayEffectModCallbackData`가 포함된 `FOnAttributeChangeData` 파라미터를 제공합니다. **주의:** `FGameplayEffectModCallbackData`는 서버에서만 설정됩니다.

```c++
AbilitySystemComponent->GetGameplayAttributeValueChangeDelegate(AttributeSetBase->GetHealthAttribute()).AddUObject(this, &AGDPlayerState::HealthChanged);
```

```c++
virtual void HealthChanged(const FOnAttributeChangeData& Data);
```

샘플 프로젝트는 HUD를 업데이트하고 체력이 0에 도달했을 때 플레이어 사망에 응답하기 위해 `GDPlayerState`에서 `Attribute` 값 변경 델리게이트에 바인딩합니다.

이를 `ASyncTask`로 감싸는 사용자 정의 블루프린트 노드가 샘플 프로젝트에 포함되어 있습니다. 이는 `UI_HUD` UMG 위젯에서 체력, 마나, 스태미나 값을 업데이트하는 데 사용됩니다. 이 `AsyncTask`는 수동으로 `EndTask()`를 호출할 때까지 영원히 살아있을 것입니다. 우리는 UMG 위젯의 `Destruct` 이벤트에서 이를 수행합니다. `AsyncTaskAttributeChanged.h/cpp`를 참조하세요.

![어트리뷰트 변경 듣기 BP 노드](https://github.com/tranek/GASDocumentation/raw/master/Images/attributechange.png)

**[⬆ Back to Top](#table-of-contents)**

<a name="concepts-a-derived"></a>
#### 4.3.5 파생 어트리뷰트
하나 이상의 다른 `Attributes`로부터 일부 또는 전체 값이 파생되는 `Attribute`를 만들려면, 하나 이상의 `Attribute Based` 또는 [`MMC`](#concepts-ge-mmc) [`Modifiers`](#concepts-ge-mods)가 있는 `Infinite` `GameplayEffect`를 사용하세요. `Derived Attribute`는 의존하는 `Attribute`가 업데이트될 때 자동으로 업데이트됩니다.

`Derived Attribute`의 모든 `Modifiers`에 대한 최종 공식은 `Modifier Aggregators`의 공식과 동일합니다. 계산이 특정 순서로 이루어져야 한다면, `MMC` 내에서 모두 수행하세요.

```
((CurrentValue + Additive) * Multiplicitive) / Division
```

**주의:** PIE에서 여러 클라이언트로 플레이하는 경우, 첫 번째 클라이언트 외의 다른 클라이언트에서 독립적인 `Attributes`가 업데이트될 때 `Derived Attributes`가 업데이트되지 않으므로 편집기 기본 설정에서 `Run Under One Process`를 비활성화해야 합니다.

이 예시에서는 `TestAttrB`와 `TestAttrC` `Attributes`로부터 `TestAttrA`의 값을 도출하는 `Infinite` `GameplayEffect`가 있습니다. 공식은 `TestAttrA = (TestAttrA + TestAttrB) * ( 2 * TestAttrC)`입니다. `TestAttrA`는 `Attributes` 중 하나라도 값이 업데이트될 때마다 자동으로 값을 재계산합니다.

![파생 어트리뷰트 예시](https://github.com/tranek/GASDocumentation/raw/master/Images/derivedattribute.png)

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-as"></a>
### 4.4 어트리뷰트 세트

<a name="concepts-as-definition"></a>
#### 4.4.1 어트리뷰트 세트 정의
`AttributeSet`는 `Attributes`를 정의하고, 보유하며, 변경을 관리합니다. 개발자는 [`UAttributeSet`](https://docs.unrealengine.com/en-US/API/Plugins/GameplayAbilities/UAttributeSet/index.html)에서 서브클래스를 만들어야 합니다. `OwnerActor`의 생성자에서 `AttributeSet`를 생성하면 자동으로 ASC에 등록됩니다. **이는 반드시 C++에서 수행해야 합니다**.

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-as-design"></a>
#### 4.4.2 어트리뷰트 세트 설계
`ASC`는 하나 또는 여러 개의 `AttributeSets`를 가질 수 있습니다. AttributeSets는 무시할 만한 메모리 오버헤드를 가지므로 얼마나 많은 `AttributeSets`를 사용할지는 개발자의 조직적 결정에 달려 있습니다.

게임의 모든 `Actor`가 공유하는 하나의 큰 단일 `AttributeSet`를 가지고 필요한 경우에만 어트리뷰트를 사용하고 사용하지 않는 어트리뷰트는 무시하는 것도 acceptable합니다.

또는 `Attributes`의 그룹을 나타내는 둘 이상의 `AttributeSet`를 가지고 필요에 따라 `Actors`에 선택적으로 추가할 수 있습니다. 예를 들어, 체력 관련 `Attributes`를 위한 `AttributeSet`, 마나 관련 `Attributes`를 위한 `AttributeSet` 등을 가질 수 있습니다. MOBA 게임에서 영웅은 마나가 필요할 수 있지만 미니언은 필요하지 않을 수 있습니다. 따라서 영웅은 마나 `AttributeSet`를 얻고 미니언은 얻지 않을 것입니다.

또한, `AttributeSets`는 `Actor`가 가지는 `Attributes`를 선택적으로 선택하는 또 다른 방법으로 서브클래스화될 수 있습니다. `Attributes`는 내부적으로 `AttributeSetClassName.AttributeName`으로 참조됩니다. `AttributeSet`를 서브클래스화할 때, 부모 클래스의 모든 `Attributes`는 여전히 부모 클래스의 이름을 접두사로 가집니다.

둘 이상의 `AttributeSet`를 가질 수 있지만, `ASC`에 동일한 클래스의 `AttributeSet`를 둘 이상 가지면 안 됩니다. 동일한 클래스에서 둘 이상의 `AttributeSet`가 있으면 어떤 `AttributeSet`를 사용할지 모르고 그냥 하나를 선택할 것입니다.

<a name="concepts-as-design-subcomponents"></a>
##### 4.4.2.1 개별 어트리뷰트가 있는 서브컴포넌트
개별적으로 데미지를 받을 수 있는 방어구 조각과 같이 `Pawn`에 여러 개의 데미지를 받을 수 있는 컴포넌트가 있는 시나리오에서, `Pawn`이 가질 수 있는 최대 데미지 가능 컴포넌트 수를 알고 있다면 하나의 `AttributeSet`에 그만큼의 체력 `Attributes`를 만드는 것을 권장합니다 - DamageableCompHealth0, DamageableCompHealth1 등으로 데미지 가능 컴포넌트의 논리적 '슬롯'을 나타냅니다. 데미지 가능 컴포넌트 클래스 인스턴스에서 `GameplayAbilities`나 [`Executions`](#concepts-ge-ec)가 어떤 `Attribute`에 데미지를 적용할지 알 수 있는 슬롯 번호 `Attribute`를 할당합니다. 최대 수보다 적거나 0개의 데미지 가능 컴포넌트를 가진 `Pawns`도 괜찮습니다. `AttributeSet`가 `Attribute`를 가지고 있다고 해서 반드시 사용해야 하는 것은 아닙니다. 사용되지 않는 `Attributes`는 무시할 만한 양의 메모리를 차지합니다.

서브컴포넌트가 각각 많은 `Attributes`를 필요로 하거나, 잠재적으로 무한한 수의 서브컴포넌트가 있거나, 서브컴포넌트가 분리되어 다른 플레이어가 사용할 수 있거나(예: 무기), 또는 다른 이유로 이 접근 방식이 적합하지 않다면, `Attributes` 대신 컴포넌트에 일반 float를 저장하는 것을 권장합니다. [Item Attributes](#concepts-as-design-itemattributes)를 참조하세요.

<a name="concepts-as-design-addremoveruntime"></a>
##### 4.4.2.2 런타임에 AttributeSets 추가 및 제거
`AttributeSets`는 런타임에 `ASC`에 추가하고 제거할 수 있습니다; 그러나 `AttributeSets`를 제거하는 것은 위험할 수 있습니다. 예를 들어, 서버보다 먼저 클라이언트에서 `AttributeSet`가 제거되고 `Attribute` 값 변경이 클라이언트에 복제되면, `Attribute`가 자신의 `AttributeSet`를 찾지 못하고 게임이 충돌할 수 있습니다.

인벤토리에 무기 추가 시:
```c++
AbilitySystemComponent->GetSpawnedAttributes_Mutable().AddUnique(WeaponAttributeSetPointer);
AbilitySystemComponent->ForceReplication();
```

인벤토리에서 무기 제거 시:
```c++
AbilitySystemComponent->GetSpawnedAttributes_Mutable().Remove(WeaponAttributeSetPointer);
AbilitySystemComponent->ForceReplication();
```

<a name="concepts-as-design-itemattributes"></a>
##### 4.4.2.3 아이템 어트리뷰트 (무기 탄약)
`Attributes`가 있는 장착 가능한 아이템(무기 탄약, 방어구 내구도 등)을 구현하는 방법은 몇 가지가 있습니다. 이러한 모든 접근 방식은 값을 직접 아이템에 저장합니다. 이는 수명 동안 둘 이상의 플레이어가 장착할 수 있는 아이템에 필요합니다.

> 1. 아이템에 일반 float 사용 (**권장**)
> 2. 아이템에 별도의 `AttributeSet` 사용
> 3. 아이템에 별도의 `ASC` 사용

<a name="concepts-as-design-itemattributes-plainfloats"></a>
###### 4.4.2.3.1 아이템에 일반 Float 사용
`Attributes` 대신, 아이템 클래스 인스턴스에 일반 float 값을 저장합니다. Fortnite와 [GASShooter](https://github.com/tranek/GASShooter)는 이 방식으로 총기 탄약을 처리합니다. 총의 경우, 최대 클립 크기, 현재 클립의 탄약, 예비 탄약 등을 총 인스턴스에 직접 복제된 float(`COND_OwnerOnly`)로 저장합니다. 무기가 예비 탄약을 공유한다면, 예비 탄약을 공유 탄약 `AttributeSet`의 `Attribute`로 캐릭터에 이동시킬 수 있습니다(재장전 능력은 `Cost GE`를 사용하여 예비 탄약에서 총의 float 클립 탄약으로 가져올 수 있습니다). 현재 클립 탄약에 `Attributes`를 사용하지 않으므로, 총의 float에 대한 비용을 확인하고 적용하기 위해 `UGameplayAbility`의 일부 함수를 오버라이드해야 합니다. 능력을 부여할 때 [`GameplayAbilitySpec`](https://github.com/tranek/GASDocumentation#concepts-ga-spec)에서 총을 `SourceObject`로 만들면 능력 내부에서 능력을 부여한 총에 접근할 수 있습니다.

자동 발사 중 총이 탄약 양을 다시 복제하고 로컬 탄약 양을 덮어쓰는 것을 방지하기 위해, `PreReplication()`에서 플레이어가 `IsFiring` `GameplayTag`를 가지고 있는 동안 복제를 비활성화합니다. 여기서 기본적으로 자체 로컬 예측을 수행하고 있습니다.

```c++
void AGSWeapon::PreReplication(IRepChangedPropertyTracker& ChangedPropertyTracker)
{
	Super::PreReplication(ChangedPropertyTracker);

	DOREPLIFETIME_ACTIVE_OVERRIDE(AGSWeapon, PrimaryClipAmmo, (IsValid(AbilitySystemComponent) && !AbilitySystemComponent->HasMatchingGameplayTag(WeaponIsFiringTag)));
	DOREPLIFETIME_ACTIVE_OVERRIDE(AGSWeapon, SecondaryClipAmmo, (IsValid(AbilitySystemComponent) && !AbilitySystemComponent->HasMatchingGameplayTag(WeaponIsFiringTag)));
}
```

이점:
1. `AttributeSets` 사용의 제한 사항을 피할 수 있습니다 (아래 참조)

제한 사항:
1. 기존 `GameplayEffect` 워크플로우를 사용할 수 없습니다 (탄약 사용을 위한 `Cost GEs` 등)
2. 총의 float에 대한 탄약 비용을 확인하고 적용하기 위해 `UGameplayAbility`의 주요 함수를 오버라이드하는 작업이 필요합니다

<a name="concepts-as-design-itemattributes-attributeset"></a>
###### 4.4.2.3.2 아이템에 `AttributeSet` 사용
아이템에 별도의 `AttributeSet`를 사용하여 [플레이어의 인벤토리에 추가할 때 플레이어의 `ASC`에 추가](#concepts-as-design-addremoveruntime)하는 방식은 작동할 수 있지만 몇 가지 주요 제한 사항이 있습니다. [GASShooter](https://github.com/tranek/GASShooter)의 초기 버전에서 무기 탄약에 대해 이 방식을 사용했습니다. 무기는 최대 클립 크기, 현재 클립의 탄약, 예비 탄약 등과 같은 `Attributes`를 무기 클래스에 있는 `AttributeSet`에 저장합니다. 무기가 예비 탄약을 공유한다면, 예비 탄약을 공유 탄약 `AttributeSet`의 캐릭터로 이동시킬 수 있습니다. 서버에서 플레이어의 인벤토리에 무기가 추가될 때, 무기는 자신의 `AttributeSet`를 플레이어의 `ASC::SpawnedAttributes`에 추가합니다. 그런 다음 서버는 이를 클라이언트로 복제합니다. 인벤토리에서 무기가 제거되면 `ASC::SpawnedAttributes`에서 해당 `AttributeSet`를 제거합니다.

`AttributeSet`가 `OwnerActor`가 아닌 다른 곳(예: 무기)에 있을 때, 처음에 `AttributeSet`에서 컴파일 오류가 발생할 수 있습니다. 해결 방법은 생성자 대신 `BeginPlay()`에서 `AttributeSet`를 구성하고 무기에 `IAbilitySystemInterface`를 구현하는 것입니다(플레이어 인벤토리에 무기를 추가할 때 `ASC`에 대한 포인터를 설정).

```c++
void AGSWeapon::BeginPlay()
{
	if (!AttributeSet)
	{
		AttributeSet = NewObject<UGSWeaponAttributeSet>(this);
	}
	//...
}
```

이 [GASShooter의 이전 버전](https://github.com/tranek/GASShooter/tree/df5949d0dd992bd3d76d4a728f370f2e2c827735)에서 실제 사용 예를 확인할 수 있습니다.

이점:
1. 기존 `GameplayAbility` 및 `GameplayEffect` 워크플로우를 사용할 수 있습니다 (탄약 사용을 위한 `Cost GEs` 등)
2. 매우 적은 수의 아이템에 대해 설정하기 간단합니다

제한 사항:
1. 모든 무기 유형에 대해 새로운 `AttributeSet` 클래스를 만들어야 합니다. `ASC`는 기능적으로 클래스당 하나의 `AttributeSet` 인스턴스만 가질 수 있습니다. `Attribute`의 변경 사항은 `ASC`의 `SpawnedAttributes` 배열에서 해당 `AttributeSet` 클래스의 첫 번째 인스턴스만 찾기 때문입니다. 동일한 `AttributeSet` 클래스의 추가 인스턴스는 무시됩니다.
2. 이전 이유로 인해 플레이어의 인벤토리에 각 유형의 무기를 하나씩만 가질 수 있습니다.
3. `AttributeSet`를 제거하는 것은 위험합니다. GASShooter에서 플레이어가 로켓으로 자살했을 때, 플레이어는 즉시 인벤토리에서 로켓 발사기를 제거했습니다(이는 `ASC`에서 해당 `AttributeSet`도 제거합니다). 서버가 로켓 발사기의 탄약 `Attribute`가 변경되었다고 복제했을 때, 클라이언트의 `ASC`에 `AttributeSet`가 더 이상 존재하지 않아 게임이 충돌했습니다.

<a name="concepts-as-design-itemattributes-asc"></a>
###### 4.4.2.3.3 아이템에 `ASC` 사용
각 아이템에 전체 `AbilitySystemComponent`를 두는 것은 극단적인 접근 방식입니다. 저는 개인적으로 이 방식을 사용해본 적이 없으며, 실제 사용 사례도 본 적이 없습니다. 이를 작동시키려면 많은 엔지니어링 작업이 필요할 것입니다.

> 동일한 소유자를 가지지만 다른 아바타를 가진 여러 AbilitySystemComponents를 가지는 것이 가능한가요 (예: 폰과 무기/아이템/발사체에 PlayerState를 Owner로 설정)?
> 
> 제가 보는 첫 번째 문제는 소유 액터에 IGameplayTagAssetInterface와 IAbilitySystemInterface를 구현하는 것입니다. 전자는 가능할 수 있습니다: 모든 ASC에서 태그를 집계하면 됩니다(그러나 주의하세요 - HasAllMatchingGameplayTags는 ASC 간 집계를 통해서만 충족될 수 있습니다. 단순히 각 ASC에 호출을 전달하고 결과를 OR 연산하는 것으로는 충분하지 않을 것입니다). 하지만 후자는 더 까다롭습니다: 어떤 ASC가 권위 있는 것인가요? 누군가 GE를 적용하려고 할 때 - 어떤 ASC가 이를 받아야 할까요? 이러한 문제를 해결할 수 있을지 모르지만 이 부분이 가장 어려울 것입니다: 소유자는 여러 개의 ASC를 가지게 될 것입니다.
> 
> 폰과 무기에 별도의 ASC를 두는 것은 그 자체로 의미가 있을 수 있습니다. 예를 들어, 무기를 설명하는 태그와 소유 폰을 설명하는 태그를 구분할 수 있습니다. 무기에 부여된 태그가 소유자에게도 "적용"되고 다른 것에는 적용되지 않는 것이 의미가 있을 수 있습니다(예: 속성과 GE는 독립적이지만 소유자는 위에서 설명한 대로 소유한 태그를 집계합니다). 이는 작동할 수 있을 것 같습니다. 하지만 동일한 소유자를 가진 여러 ASC를 두는 것은 복잡해질 수 있습니다.

*Epic의 Dave Ratti가 [커뮤니티 질문 #6](https://epicgames.ent.box.com/s/m1egifkxv3he3u3xezb9hzbgroxyhx89)에 대한 답변*

이점:
1. 기존 `GameplayAbility` 및 `GameplayEffect` 워크플로우를 사용할 수 있습니다 (탄약 사용을 위한 `Cost GEs` 등)
2. `AttributeSet` 클래스를 재사용할 수 있습니다 (각 무기의 ASC에 하나씩)

제한 사항:
1. 알 수 없는 엔지니어링 비용
2. 과연 가능한가?

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-as-attributes"></a>
#### 4.4.3 어트리뷰트 정의
**`Attributes`는 C++에서만 정의할 수 있습니다** `AttributeSet`의 헤더 파일에서 정의합니다. 모든 `AttributeSet` 헤더 파일의 상단에 다음 매크로 블록을 추가하는 것이 좋습니다. 이는 `Attributes`에 대한 getter와 setter 함수를 자동으로 생성합니다.

```c++
// AttributeSet.h의 매크로 사용
#define ATTRIBUTE_ACCESSORS(ClassName, PropertyName) \
	GAMEPLAYATTRIBUTE_PROPERTY_GETTER(ClassName, PropertyName) \
	GAMEPLAYATTRIBUTE_VALUE_GETTER(PropertyName) \
	GAMEPLAYATTRIBUTE_VALUE_SETTER(PropertyName) \
	GAMEPLAYATTRIBUTE_VALUE_INITTER(PropertyName)
```

복제된 체력 어트리뷰트는 다음과 같이 정의됩니다:

```c++
UPROPERTY(BlueprintReadOnly, Category = "Health", ReplicatedUsing = OnRep_Health)
FGameplayAttributeData Health;
ATTRIBUTE_ACCESSORS(UGDAttributeSetBase, Health)
```

헤더에 `OnRep` 함수도 정의합니다:
```c++
UFUNCTION()
virtual void OnRep_Health(const FGameplayAttributeData& OldHealth);
```

`AttributeSet`의 .cpp 파일은 예측 시스템에서 사용하는 `GAMEPLAYATTRIBUTE_REPNOTIFY` 매크로로 `OnRep` 함수를 채워야 합니다:
```c++
void UGDAttributeSetBase::OnRep_Health(const FGameplayAttributeData& OldHealth)
{
	GAMEPLAYATTRIBUTE_REPNOTIFY(UGDAttributeSetBase, Health, OldHealth);
}
```

마지막으로, `Attribute`를 `GetLifetimeReplicatedProps`에 추가해야 합니다:
```c++
void UGDAttributeSetBase::GetLifetimeReplicatedProps(TArray<FLifetimeProperty>& OutLifetimeProps) const
{
	Super::GetLifetimeReplicatedProps(OutLifetimeProps);

	DOREPLIFETIME_CONDITION_NOTIFY(UGDAttributeSetBase, Health, COND_None, REPNOTIFY_Always);
}
```

`REPNOTIFY_Always`는 로컬 값이 이미 서버에서 복제된 값과 같을 때(예측으로 인해) `OnRep` 함수를 트리거하도록 지시합니다. 기본적으로 로컬 값이 서버에서 복제된 값과 같으면 `OnRep` 함수를 트리거하지 않습니다.

`Attribute`가 `Meta Attribute`처럼 복제되지 않는 경우, `OnRep`와 `GetLifetimeReplicatedProps` 단계는 건너뛸 수 있습니다.

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-as-init"></a>
#### 4.4.4 어트리뷰트 초기화
`Attributes`를 초기화하는 방법(즉, `BaseValue`와 결과적으로 `CurrentValue`를 초기값으로 설정하는 방법)은 여러 가지가 있습니다. Epic은 즉시 `GameplayEffect`를 사용하는 것을 권장합니다. 이 방법은 샘플 프로젝트에서도 사용됩니다.

`Attributes`를 초기화하기 위한 즉시 `GameplayEffect`를 만드는 방법은 샘플 프로젝트의 `GE_HeroAttributes` 블루프린트를 참조하세요. 이 `GameplayEffect`의 적용은 C++에서 이루어집니다.

`Attributes`를 정의할 때 `ATTRIBUTE_ACCESSORS` 매크로를 사용했다면, 각 `Attribute`에 대해 C++에서 원하는 대로 호출할 수 있는 초기화 함수가 `AttributeSet`에 자동으로 생성됩니다.

```c++
// InitHealth(float InitialValue)는 `ATTRIBUTE_ACCESSORS` 매크로로 정의된 'Health' Attribute에 대해 자동 생성된 함수입니다
AttributeSet->InitHealth(100.0f);
```

`Attributes`를 초기화하는 더 많은 방법은 `AttributeSet.h`를 참조하세요.

**참고:** 4.24 이전 버전에서는 `FAttributeSetInitterDiscreteLevels`가 `FGameplayAttributeData`와 작동하지 않았습니다. 이는 `Attributes`가 raw float였을 때 생성되었고 `FGameplayAttributeData`가 `Plain Old Data`(`POD`)가 아니라고 불평합니다. 이 문제는 4.24 버전에서 수정되었습니다 https://issues.unrealengine.com/issue/UE-76557.

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-as-preattributechange"></a>
#### 4.4.5 PreAttributeChange()
`PreAttributeChange(const FGameplayAttribute& Attribute, float& NewValue)`는 `AttributeSet`에서 `Attribute`의 `CurrentValue` 변경에 대응하는 주요 함수 중 하나로, 변경이 발생하기 전에 호출됩니다. 이는 참조 매개변수 `NewValue`를 통해 `CurrentValue`에 대한 들어오는 변경을 제한하는 이상적인 위치입니다.

예를 들어, 샘플 프로젝트에서는 이동 속도 수정자를 다음과 같이 제한합니다:
```c++
if (Attribute == GetMoveSpeedAttribute())
{
	// 150 units/s 미만으로 느려지거나 1000 units/s 이상으로 빨라질 수 없음
	NewValue = FMath::Clamp<float>(NewValue, 150, 1000);
}
```
`GetMoveSpeedAttribute()` 함수는 `AttributeSet.h`에 추가한 매크로 블록에 의해 생성됩니다 ([어트리뷰트 정의](#concepts-as-attributes) 참조).

이는 `Attribute` 설정자(AttributeSet.h의 매크로 블록에 의해 정의됨, [어트리뷰트 정의](#concepts-as-attributes) 참조)를 사용하거나 [`GameplayEffects`](#concepts-ge)를 사용하는 등 `Attributes`에 대한 모든 변경에서 트리거됩니다.

**참고:** 여기서 발생하는 제한은 `ASC`의 수정자를 영구적으로 변경하지 않습니다. 단지 수정자를 쿼리할 때 반환되는 값만 변경합니다. 이는 [`GameplayEffectExecutionCalculations`](#concepts-ge-ec)와 [`ModifierMagnitudeCalculations`](#concepts-ge-mmc)와 같이 모든 수정자로부터 `CurrentValue`를 재계산하는 것들은 제한을 다시 구현해야 한다는 것을 의미합니다.

**참고:** Epic의 `PreAttributeChange()` 주석에 따르면 이를 게임플레이 이벤트에 사용하지 말고 주로 제한에 사용하라고 합니다. `Attribute` 변경에 대한 게임플레이 이벤트의 권장 위치는 `UAbilitySystemComponent::GetGameplayAttributeValueChangeDelegate(FGameplayAttribute Attribute)`입니다 ([어트리뷰트 변경에 대한 응답](#concepts-a-changes) 참조).

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-as-postgameplayeffectexecute"></a>
#### 4.4.6 PostGameplayEffectExecute()
`PostGameplayEffectExecute(const FGameplayEffectModCallbackData & Data)`는 즉시 [`GameplayEffect`](#concepts-ge)에 의한 `Attribute`의 `BaseValue` 변경 후에만 트리거됩니다. 이는 `GameplayEffect`에 의해 변경될 때 더 많은 `Attribute` 조작을 수행하는 적절한 장소입니다.

예를 들어, 샘플 프로젝트에서는 여기서 최종 데미지 `Meta Attribute`를 체력 `Attribute`에서 차감합니다. 방어막 `Attribute`가 있다면, 체력에서 차감하기 전에 먼저 방어막에서 데미지를 차감할 것입니다. 샘플 프로젝트는 또한 이 위치를 사용하여 히트 반응 애니메이션을 적용하고, 부동 데미지 숫자를 표시하며, 경험치와 골드 보상을 킬러에게 할당합니다. 설계상, 데미지 `Meta Attribute`는 항상 즉시 `GameplayEffect`를 통해 오며 절대 `Attribute` 설정자를 통해 오지 않습니다.

마나와 스태미나와 같이 즉시 `GameplayEffects`에 의해서만 `BaseValue`가 변경되는 다른 `Attributes`도 여기서 최대값 카운터파트 `Attributes`로 제한될 수 있습니다.

**참고:** `PostGameplayEffectExecute()`가 호출될 때, `Attribute`에 대한 변경은 이미 발생했지만 아직 클라이언트에 복제되지 않았으므로 여기서 값을 제한하면 클라이언트에 두 번의 네트워크 업데이트가 발생하지 않습니다. 클라이언트는 제한 후에만 업데이트를 받게 됩니다.

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-as-onattributeaggregatorcreated"></a>
#### 4.4.7 OnAttributeAggregatorCreated()
`OnAttributeAggregatorCreated(const FGameplayAttribute& Attribute, FAggregator* NewAggregator)`는 이 세트의 `Attribute`에 대한 `Aggregator`가 생성될 때 트리거됩니다. 이는 [`FAggregatorEvaluateMetaData`](https://docs.unrealengine.com/en-US/API/Plugins/GameplayAbilities/FAggregatorEvaluateMetaData/index.html)의 사용자 정의 설정을 허용합니다. `AggregatorEvaluateMetaData`는 `Aggregator`가 적용된 모든 [`Modifiers`](#concepts-ge-mods)를 기반으로 `Attribute`의 `CurrentValue`를 평가하는 데 사용됩니다. 기본적으로 `AggregatorEvaluateMetaData`는 `Aggregator`에 의해 어떤 `Modifiers`가 적격한지 결정하는 데만 사용되며, `MostNegativeMod_AllPositiveMods`의 예시를 들 수 있습니다. 이는 모든 양수 `Modifiers`를 허용하지만 음수 `Modifiers`는 가장 음수인 것만 제한합니다. 이는 Paragon에서 플레이어에게 얼마나 많은 감속 효과가 있든 상관없이 가장 음수인 이동 속도 감속 효과만 적용하면서 모든 양수 이동 속도 버프를 적용하는 데 사용되었습니다. 적격하지 않은 `Modifiers`는 여전히 `ASC`에 존재하지만, 최종 `CurrentValue`에 집계되지 않습니다. 조건이 변경되면 나중에 잠재적으로 적격해질 수 있습니다. 예를 들어 가장 음수인 `Modifier`가 만료되면 다음으로 가장 음수인 `Modifier`(존재한다면)가 적격해집니다.

가장 음수인 `Modifier`와 모든 양수 `Modifiers`만 허용하는 예시에서 AggregatorEvaluateMetaData를 사용하려면:

```c++
virtual void OnAttributeAggregatorCreated(const FGameplayAttribute& Attribute, FAggregator* NewAggregator) const override;
```

```c++
void UGSAttributeSetBase::OnAttributeAggregatorCreated(const FGameplayAttribute& Attribute, FAggregator* NewAggregator) const
{
	Super::OnAttributeAggregatorCreated(Attribute, NewAggregator);

	if (!NewAggregator)
	{
		return;
	}

	if (Attribute == GetMoveSpeedAttribute())
	{
		NewAggregator->EvaluationMetaData = &FAggregatorEvaluateMetaDataLibrary::MostNegativeMod_AllPositiveMods;
	}
}
```

적격자에 대한 사용자 정의 `AggregatorEvaluateMetaData`는 `FAggregatorEvaluateMetaDataLibrary`에 정적 변수로 추가해야 합니다.

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-ge"></a>
### 4.5 게임플레이 이펙트

<a name="concepts-ge-definition"></a>
#### 4.5.1 게임플레이 이펙트 정의
[`GameplayEffects`](https://docs.unrealengine.com/en-US/API/Plugins/GameplayAbilities/UGameplayEffect/index.html) (`GE`)는 능력이 자신과 다른 대상의 [`Attributes`](#concepts-a)와 [`GameplayTags`](#concepts-gt)를 변경하는 수단입니다. 이들은 데미지나 힐링과 같은 즉각적인 `Attribute` 변경을 일으키거나 이동 속도 증가나 기절과 같은 장기적인 상태 버프/디버프를 적용할 수 있습니다. `UGameplayEffect` 클래스는 단일 게임플레이 이펙트를 정의하는 **데이터 전용** 클래스로 설계되었습니다. `GameplayEffects`에 추가적인 로직을 넣어서는 안 됩니다. 일반적으로 디자이너들은 `UGameplayEffect`의 많은 블루프린트 자식 클래스를 만들게 됩니다.

`GameplayEffects`는 [`Modifiers`](#concepts-ge-mods)와 [`Executions` (`GameplayEffectExecutionCalculation`)](#concepts-ge-ec)를 통해 `Attributes`를 변경합니다.

`GameplayEffects`는 세 가지 유형의 지속 시간을 가집니다: `Instant`, `Duration`, `Infinite`.

추가로, `GameplayEffects`는 [`GameplayCues`](#concepts-gc)를 추가/실행할 수 있습니다. `Instant` `GameplayEffect`는 `GameplayCue` `GameplayTags`에 대해 `Execute`를 호출하는 반면, `Duration` 또는 `Infinite` `GameplayEffect`는 `GameplayCue` `GameplayTags`에 대해 `Add`와 `Remove`를 호출합니다.

| 지속 시간 유형 | GameplayCue 이벤트 | 사용 시기                                                                                                                                                                                                                               |
| --------------- | ------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Instant`       | Execute             | `Attribute`의 `BaseValue`에 대한 즉각적이고 영구적인 변경을 위해 사용합니다. `GameplayTags`는 적용되지 않으며, 한 프레임 동안도 적용되지 않습니다.                                                                                      |
| `Duration`      | Add & Remove        | `Attribute`의 `CurrentValue`에 대한 일시적인 변경과 `GameplayEffect`가 만료되거나 수동으로 제거될 때 제거될 `GameplayTags`를 적용하기 위해 사용합니다. 지속 시간은 `UGameplayEffect` 클래스/블루프린트에서 지정됩니다.                  |
| `Infinite`      | Add & Remove        | `Attribute`의 `CurrentValue`에 대한 일시적인 변경과 `GameplayEffect`가 제거될 때 제거될 `GameplayTags`를 적용하기 위해 사용합니다. 이들은 스스로 만료되지 않으며 반드시 능력이나 `ASC`에 의해 수동으로 제거되어야 합니다.               |

`Duration`과 `Infinite` `GameplayEffects`는 `Period`로 정의된 매 `X`초마다 `Modifiers`와 `Executions`를 적용하는 `Periodic Effects` 옵션을 가집니다. `Periodic Effects`는 `Attribute`의 `BaseValue`를 변경하고 `GameplayCues`를 `Execute`하는 면에서 `Instant` `GameplayEffects`로 취급됩니다. 이들은 시간에 따른 데미지(DOT) 유형의 효과에 유용합니다. **참고:** `Periodic Effects`는 [예측](#concepts-p)될 수 없습니다.

`Duration`과 `Infinite` `GameplayEffects`는 적용 후 `Ongoing Tag Requirements`가 충족되지 않거나 충족될 때 일시적으로 켜고 끌 수 있습니다([게임플레이 이펙트 태그](#concepts-ge-tags)). `GameplayEffect`를 끄면 `Modifiers`의 효과와 적용된 `GameplayTags`가 제거되지만 `GameplayEffect`는 제거되지 않습니다. `GameplayEffect`를 다시 켜면 `Modifiers`와 `GameplayTags`가 다시 적용됩니다.

`Duration` 또는 `Infinite` `GameplayEffect`의 `Modifiers`를 수동으로 재계산해야 하는 경우(`Attributes`에서 오지 않는 데이터를 사용하는 `MMC`가 있는 경우 등), `UAbilitySystemComponent::ActiveGameplayEffects.SetActiveGameplayEffectLevel(FActiveGameplayEffectHandle ActiveHandle, int32 NewLevel)`을 `UAbilitySystemComponent::ActiveGameplayEffects.GetActiveGameplayEffect(ActiveHandle).Spec.GetLevel()`을 사용하여 이미 가지고 있는 동일한 레벨로 호출할 수 있습니다. 기반 `Attributes`에 기반한 `Modifiers`는 해당 기반 `Attributes`가 업데이트될 때 자동으로 업데이트됩니다. `Modifiers`를 업데이트하기 위한 `SetActiveGameplayEffectLevel()`의 주요 함수는 다음과 같습니다:

```C++
MarkItemDirty(Effect);
Effect.Spec.CalculateModifierMagnitudes();
// 그렇지 않으면 이미 있는 레벨로 설정할 필요 없이 이 세 함수를 호출할 것입니다
UpdateAllAggregatorModMagnitudes(Effect);
```

`GameplayEffects`는 일반적으로 인스턴스화되지 않습니다. 능력이나 `ASC`가 `GameplayEffect`를 적용하고자 할 때, `GameplayEffect`의 `ClassDefaultObject`에서 [`GameplayEffectSpec`](#concepts-ge-spec)을 생성합니다. 성공적으로 적용된 `GameplayEffectSpecs`는 `FActiveGameplayEffect`라는 새로운 구조체에 추가되며, 이는 `ASC`가 `ActiveGameplayEffects`라는 특별한 컨테이너 구조체에서 추적하는 것입니다.

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-ge-applying"></a>
#### 4.5.2 게임플레이 이펙트 적용
`GameplayEffects`는 [`GameplayAbilities`](#concepts-ga)의 함수와 `ASC`의 함수를 통해 여러 가지 방법으로 적용될 수 있으며, 일반적으로 `ApplyGameplayEffectTo` 형태를 취합니다. 다양한 함수들은 본질적으로 최종적으로 `Target`에 대해 `UAbilitySystemComponent::ApplyGameplayEffectSpecToSelf()`를 호출하는 편의 함수입니다.

`GameplayAbility` 외부에서 `GameplayEffects`를 적용하려면, 예를 들어 발사체에서, `Target`의 `ASC`를 얻고 `ApplyGameplayEffectToSelf` 함수 중 하나를 사용해야 합니다.

`ASC`에 `Duration` 또는 `Infinite` `GameplayEffects`가 적용될 때 이를 수신하려면 델리게이트에 바인딩할 수 있습니다:
```c++
AbilitySystemComponent->OnActiveGameplayEffectAddedDelegateToSelf.AddUObject(this, &APACharacterBase::OnActiveGameplayEffectAddedCallback);
```
콜백 함수:
```c++
virtual void OnActiveGameplayEffectAddedCallback(UAbilitySystemComponent* Target, const FGameplayEffectSpec& SpecApplied, FActiveGameplayEffectHandle ActiveHandle);
```

서버는 복제 모드에 관계없이 항상 이 함수를 호출합니다. 자율 프록시는 `Full`과 `Mixed` 복제 모드에서 복제된 `GameplayEffects`에 대해서만 이를 호출합니다. 시뮬레이트된 프록시는 `Full` [복제 모드](#concepts-asc-rm)에서만 이를 호출합니다.

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-ga-removing"></a>
#### 4.5.3 게임플레이 이펙트 제거
`GameplayEffects`는 [`GameplayAbilities`](#concepts-ga)의 함수와 `ASC`의 함수를 통해 여러 가지 방법으로 제거될 수 있으며, 일반적으로 `RemoveActiveGameplayEffect` 형태를 취합니다. 다양한 함수들은 본질적으로 최종적으로 `Target`에 대해 `FActiveGameplayEffectsContainer::RemoveActiveEffects()`를 호출하는 편의 함수입니다.

`GameplayAbility` 외부에서 `GameplayEffects`를 제거하려면, `Target`의 `ASC`를 얻고 `RemoveActiveGameplayEffect` 함수 중 하나를 사용해야 합니다.

`ASC`에서 `Duration` 또는 `Infinite` `GameplayEffects`가 제거될 때 이를 수신하려면 델리게이트에 바인딩할 수 있습니다:
```c++
AbilitySystemComponent->OnAnyGameplayEffectRemovedDelegate().AddUObject(this, &APACharacterBase::OnRemoveGameplayEffectCallback);
```
콜백 함수:
```c++
virtual void OnRemoveGameplayEffectCallback(const FActiveGameplayEffect& EffectRemoved);
```

서버는 복제 모드에 관계없이 항상 이 함수를 호출합니다. 자율 프록시는 `Full`과 `Mixed` 복제 모드에서 복제된 `GameplayEffects`에 대해서만 이를 호출합니다. 시뮬레이트된 프록시는 `Full` [복제 모드](#concepts-asc-rm)에서만 이를 호출합니다.

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-ge-mods"></a>
#### 4.5.4 게임플레이 이펙트 수정자
`Modifiers`는 `Attribute`를 변경하며, [예측적으로](#concepts-p) `Attribute`를 변경할 수 있는 유일한 방법입니다. `GameplayEffect`는 0개 또는 여러 개의 `Modifiers`를 가질 수 있습니다. 각 `Modifier`는 지정된 연산을 통해 하나의 `Attribute`만 변경하는 책임을 집니다.

| 연산       | 설명                                                                                   |
| ---------- | -------------------------------------------------------------------------------------- |
| `Add`      | 결과를 `Modifier`가 지정한 `Attribute`에 더합니다. 뺄셈을 위해서는 음수 값을 사용하세요. |
| `Multiply` | 결과를 `Modifier`가 지정한 `Attribute`에 곱합니다.                                      |
| `Divide`   | 결과로 `Modifier`가 지정한 `Attribute`를 나눕니다.                                      |
| `Override` | `Modifier`가 지정한 `Attribute`를 결과로 덮어씁니다.                                    |

`Attribute`의 `CurrentValue`는 모든 `Modifiers`의 집계 결과를 `BaseValue`에 더한 값입니다. `Modifiers`가 집계되는 방식의 공식은 `GameplayEffectAggregator.cpp`의 `FAggregatorModChannel::EvaluateWithBase`에서 다음과 같이 정의됩니다:
```c++
((InlineBaseValue + Additive) * Multiplicitive) / Division
```

`Override` `Modifiers`는 최종 값을 마지막으로 적용된 `Modifier`의 값으로 덮어씁니다.

**참고:** 백분율 기반 변경의 경우, 덧셈 후에 발생하도록 `Multiply` 연산을 사용해야 합니다.

**참고:** [예측](#concepts-p)은 백분율 변경에 문제가 있습니다.

`Modifiers`에는 네 가지 유형이 있습니다: Scalable Float, Attribute Based, Custom Calculation Class, Set By Caller. 이들은 모두 일부 float 값을 생성하며, 이 값은 `Modifier`의 연산에 기반하여 지정된 `Attribute`를 변경하는 데 사용됩니다.

| `Modifier` 유형           | 설명                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| ------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Scalable Float`          | `FScalableFloats`는 변수를 행으로, 레벨을 열로 가지는 데이터 테이블을 가리킬 수 있는 구조체입니다. Scalable Floats는 능력의 현재 레벨(또는 [`GameplayEffectSpec`](#concepts-ge-spec)에서 재정의된 경우 다른 레벨)에서 지정된 테이블 행의 값을 자동으로 읽습니다. 이 값은 계수로 추가로 조작될 수 있습니다. 데이터 테이블/행이 지정되지 않으면 값을 1로 취급하므로 계수를 사용하여 모든 레벨에서 단일 값을 하드 코딩할 수 있습니다. ![ScalableFloat](https://github.com/tranek/GASDocumentation/raw/master/Images/scalablefloats.png)                                                                                                                                                                                                                                                                                                                              |
| `Attribute Based`         | `Attribute Based` `Modifiers`는 `Source`(누가 `GameplayEffectSpec`을 생성했는지) 또는 `Target`(누가 `GameplayEffectSpec`을 받았는지)의 백업 `Attribute`의 `CurrentValue` 또는 `BaseValue`를 가져와 계수와 계수 전후 추가로 더 수정합니다. `Snapshotting`은 `GameplayEffectSpec`이 생성될 때 백업 `Attribute`가 캡처됨을 의미하며, 스냅샷을 사용하지 않으면 `GameplayEffectSpec`이 적용될 때 `Attribute`가 캡처됩니다.                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| `Custom Calculation Class` | `Custom Calculation Class`는 복잡한 `Modifiers`에 대해 가장 유연성을 제공합니다. 이 `Modifier`는 [`ModifierMagnitudeCalculation`](#concepts-ge-mmc) 클래스를 사용하고 결과 float 값을 계수와 계수 전후 추가로 더 조작할 수 있습니다.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| `Set By Caller`           | `SetByCaller` `Modifiers`는 런타임에 `GameplayEffect` 외부에서 능력이나 `GameplayEffectSpec`을 만든 사람에 의해 `GameplayEffectSpec`에 설정되는 값입니다. 예를 들어, 플레이어가 버튼을 누르고 있는 시간에 따라 데미지를 설정하려면 `SetByCaller`를 사용할 것입니다. `SetByCallers`는 본질적으로 `GameplayEffectSpec`에 존재하는 `TMap<FGameplayTag, float>`입니다. `Modifier`는 단지 `Aggregator`에게 제공된 `GameplayTag`와 연관된 `SetByCaller` 값을 찾도록 지시하는 것입니다. `Modifiers`가 사용하는 `SetByCallers`는 개념의 `GameplayTag` 버전만 사용할 수 있습니다. `FName` 버전은 여기서 비활성화됩니다. `Modifier`가 `SetByCaller`로 설정되었지만 올바른 `GameplayTag`를 가진 `SetByCaller`가 `GameplayEffectSpec`에 존재하지 않으면 게임은 런타임 오류를 발생시키고 0 값을 반환합니다. 이는 `Divide` 연산의 경우 문제를 일으킬 수 있습니다. `SetByCallers` 사용 방법에 대한 자세한 정보는 [`SetByCallers`](#concepts-ge-spec-setbycaller)를 참조하세요. |

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-ge-mods-multiplydivide"></a>
##### 4.5.4.1 곱하기 및 나누기 수정자
기본적으로 모든 `Multiply`와 `Divide` `Modifiers`는 `Attribute`의 `BaseValue`에 곱하거나 나누기 전에 함께 더해집니다.

```c++
float FAggregatorModChannel::EvaluateWithBase(float InlineBaseValue, const FAggregatorEvaluateParameters& Parameters) const
{
	...
	float Additive = SumMods(Mods[EGameplayModOp::Additive], GameplayEffectUtilities::GetModifierBiasByModifierOp(EGameplayModOp::Additive), Parameters);
	float Multiplicitive = SumMods(Mods[EGameplayModOp::Multiplicitive], GameplayEffectUtilities::GetModifierBiasByModifierOp(EGameplayModOp::Multiplicitive), Parameters);
	float Division = SumMods(Mods[EGameplayModOp::Division], GameplayEffectUtilities::GetModifierBiasByModifierOp(EGameplayModOp::Division), Parameters);
	...
	return ((InlineBaseValue + Additive) * Multiplicitive) / Division;
	...
}
```

```c++
float FAggregatorModChannel::SumMods(const TArray<FAggregatorMod>& InMods, float Bias, const FAggregatorEvaluateParameters& Parameters)
{
	float Sum = Bias;

	for (const FAggregatorMod& Mod : InMods)
	{
		if (Mod.Qualifies())
		{
			Sum += (Mod.EvaluatedMagnitude - Bias);
		}
	}

	return Sum;
}
```
*`GameplayEffectAggregator.cpp`에서*

이 공식에서 `Multiply`와 `Divide` `Modifiers` 모두 `1`의 `Bias` 값을 가집니다(`Addition`은 `0`의 `Bias`를 가짐). 따라서 다음과 같은 형태가 됩니다:

```
1 + (Mod1.Magnitude - 1) + (Mod2.Magnitude - 1) + ...
```

이 공식은 몇 가지 예상치 못한 결과를 초래합니다. 첫째, 이 공식은 모든 수정자를 `BaseValue`에 곱하거나 나누기 전에 함께 더합니다. 대부분의 사람들은 이들을 함께 곱하거나 나눌 것으로 예상할 것입니다. 예를 들어, 두 개의 `1.5` `Multiply` 수정자가 있다면, 대부분의 사람들은 `BaseValue`가 `1.5 x 1.5 = 2.25`로 곱해질 것으로 예상할 것입니다. 대신, 이는 `1.5`들을 함께 더해 `BaseValue`를 `2`로 곱합니다 (`50% 증가 + 또 다른 50% 증가 = 100% 증가`). 이는 `GameplayPrediction.h`의 예시에서 `500` 기본 속도에 `10%` 속도 버프가 적용되면 `550`이 되고, 또 다른 `10%` 속도 버프를 추가하면 `600`이 되는 것과 같습니다.

둘째, 이 공식은 Paragon을 염두에 두고 설계되어 사용할 수 있는 값에 대한 몇 가지 문서화되지 않은 규칙이 있습니다.

`Multiply`와 `Divide` 곱셈 덧셈 공식의 규칙:
* `(1보다 작은 값은 최대 하나) 그리고 ([1, 2) 범위의 값은 여러 개 가능)`
* `또는 (2 이상의 값 하나)`

공식의 `Bias`는 기본적으로 `[1, 2)` 범위의 숫자에서 정수 자릿수를 뺍니다. 첫 번째 `Modifier`의 `Bias`는 시작 `Sum` 값(루프 전에 `Bias`로 설정됨)에서 빼기 때문에 단일 값이 작동하고 `1`보다 작은 하나의 값이 `[1, 2)` 범위의 숫자와 함께 작동하는 이유입니다.

`Multiply`의 몇 가지 예:  
곱셈 인자: `0.5`  
`1 + (0.5 - 1) = 0.5`, 정확함

곱셈 인자: `0.5, 0.5`  
`1 + (0.5 - 1) + (0.5 - 1) = 0`, 부정확함, `1`을 예상했나요? `1`보다 작은 여러 값은 곱셈 인자를 더하는 데 의미가 없습니다. Paragon은 [`Multiply` `Modifiers`에 대해 가장 큰 음수 값만 사용](#cae-nonstackingge)하도록 설계되어 `BaseValue`에 곱해지는 `1`보다 작은 값은 최대 하나만 있을 수 있습니다.

곱셈 인자: `1.1, 0.5`  
`1 + (0.5 - 1) + (1.1 - 1) = 0.6`, 정확함

곱셈 인자: `5, 5`  
`1 + (5 - 1) + (5 - 1) = 9`, 부정확함, `10`을 예상했나요? 항상 `수정자의 합 - 수정자의 수 + 1`이 될 것입니다.

많은 게임에서는 `Multiply`와 `Divide` `Modifiers`가 `BaseValue`에 적용되기 전에 함께 곱하고 나누기를 원할 것입니다. 이를 달성하려면 `FAggregatorModChannel::EvaluateWithBase()` 함수의 **엔진 코드를 변경**해야 합니다.

```c++
float FAggregatorModChannel::EvaluateWithBase(float InlineBaseValue, const FAggregatorEvaluateParameters& Parameters) const
{
	...
	float Multiplicitive = MultiplyMods(Mods[EGameplayModOp::Multiplicitive], Parameters);
	float Division = MultiplyMods(Mods[EGameplayModOp::Division], Parameters);
	...

	return ((InlineBaseValue + Additive) * Multiplicitive) / Division;
}
```

```c++
float FAggregatorModChannel::MultiplyMods(const TArray<FAggregatorMod>& InMods, const FAggregatorEvaluateParameters& Parameters)
{
	float Multiplier = 1.0f;

	for (const FAggregatorMod& Mod : InMods)
	{
		if (Mod.Qualifies())
		{
			Multiplier *= Mod.EvaluatedMagnitude;
		}
	}

	return Multiplier;
}
```

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-ge-mods-gameplaytags"></a>
##### 4.5.4.2 수정자의 게임플레이 태그

각 [수정자](#concepts-ge-mods)에 대해 `SourceTags`와 `TargetTags`를 설정할 수 있습니다. 이들은 `GameplayEffect`의 [`Application Tag requirements`](#concepts-ge-tags)와 같은 방식으로 작동합니다. 따라서 태그는 효과가 적용될 때만 고려됩니다. 예를 들어, 주기적이고 무한한 효과의 경우, 이들은 효과의 첫 번째 적용 시에만 고려되며 *각 주기적 실행 시에는* 고려되지 않습니다.

`Attribute Based` 수정자는 `SourceTagFilter`와 `TargetTagFilter`도 설정할 수 있습니다. `Attribute Based` 수정자의 소스인 속성의 크기를 결정할 때, 이 필터들은 해당 속성에 대한 특정 수정자를 제외하는 데 사용됩니다. 소스나 대상이 필터의 모든 태그를 가지지 않은 수정자는 제외됩니다.

이는 구체적으로 다음을 의미합니다: 소스 ASC와 대상 ASC의 태그는 `GameplayEffects`에 의해 캡처됩니다. 소스 ASC 태그는 `GameplayEffectSpec`이 생성될 때 캡처되고, 대상 ASC 태그는 효과 실행 시 캡처됩니다. 무한 또는 지속 효과의 수정자가 적용될 자격이 있는지(즉, 그 Aggregator가 자격이 있는지) 결정할 때 이러한 필터가 설정되어 있다면, 캡처된 태그들이 필터와 비교됩니다.

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-ge-stacking"></a>
#### 4.5.5 게임플레이 이펙트 스택킹
기본적으로 `GameplayEffects`는 적용 시 이전에 존재하는 `GameplayEffectSpec` 인스턴스를 알거나 고려하지 않고 새로운 `GameplayEffectSpec` 인스턴스를 적용합니다. `GameplayEffects`는 새로운 `GameplayEffectSpec` 인스턴스가 추가되는 대신 현재 존재하는 `GameplayEffectSpec`의 스택 카운트가 변경되도록 스택으로 설정될 수 있습니다. 스택킹은 `Duration`과 `Infinite` `GameplayEffects`에서만 작동합니다.

스택킹에는 두 가지 유형이 있습니다: 소스별 집계와 대상별 집계.

| 스택킹 유형 | 설명                                                                                                       |
| ----------- | ---------------------------------------------------------------------------------------------------------- |
| 소스별 집계 | 대상에 소스 `ASC`별로 별도의 스택 인스턴스가 있습니다. 각 소스는 X 개수의 스택을 적용할 수 있습니다.       |
| 대상별 집계 | 소스에 관계없이 대상에 하나의 스택 인스턴스만 있습니다. 각 소스는 공유 스택 제한까지 스택을 적용할 수 있습니다. |

스택은 또한 만료, 지속 시간 갱신, 주기 재설정에 대한 정책을 가집니다. 이들은 `GameplayEffect` 블루프린트에서 도움이 되는 호버 툴팁을 가집니다.

샘플 프로젝트는 `GameplayEffect` 스택 변경을 수신하는 사용자 정의 블루프린트 노드를 포함합니다. HUD UMG 위젯은 이를 사용하여 플레이어가 가진 수동 방어구 스택의 양을 업데이트합니다. 이 `AsyncTask`는 수동으로 `EndTask()`가 호출될 때까지 영원히 살아있을 것이며, 우리는 UMG 위젯의 `Destruct` 이벤트에서 이를 수행합니다. `AsyncTaskEffectStackChanged.h/cpp`를 참조하세요.

![GameplayEffect 스택 변경 수신 BP 노드](https://github.com/tranek/GASDocumentation/raw/master/Images/gestackchange.png)

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-ge-ga"></a>
#### 4.5.6 부여된 능력
`GameplayEffects`는 `ASCs`에 새로운 [`GameplayAbilities`](#concepts-ga)를 부여할 수 있습니다. `Duration`과 `Infinite` `GameplayEffects`만 능력을 부여할 수 있습니다.

이에 대한 일반적인 사용 사례는 넉백이나 당김으로 다른 플레이어를 강제로 움직이게 하고 싶을 때입니다. 원하는 행동을 수행하는 자동 활성화 능력을 부여하는 `GameplayEffect`를 적용할 것입니다 (능력이 부여될 때 자동으로 활성화하는 방법은 [수동 능력](#concepts-ga-activating-passive) 참조).

디자이너는 `GameplayEffect`가 부여하는 능력, 부여할 레벨, [바인딩할 입력](#concepts-ga-input), 부여된 능력의 제거 정책을 선택할 수 있습니다.

| 제거 정책              | 설명                                                                                                                                                  |
| ---------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| 즉시 능력 취소         | 부여된 능력은 대상에서 부여한 `GameplayEffect`가 제거될 때 즉시 취소되고 제거됩니다.                                                                   |
| 종료 시 능력 제거      | 부여된 능력이 완료되도록 허용한 다음 대상에서 제거됩니다.                                                                                              |
| 아무것도 하지 않음     | 부여된 능력은 대상에서 부여한 `GameplayEffect`의 제거에 영향을 받지 않습니다. 대상은 나중에 수동으로 제거될 때까지 영구적으로 능력을 가지고 있습니다. |

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-ge-tags"></a>
#### 4.5.7 게임플레이 이펙트 태그
`GameplayEffects`는 여러 [`GameplayTagContainers`](#concepts-gt)를 가집니다. 디자이너는 각 카테고리에 대해 `Added`와 `Removed` `GameplayTagContainers`를 편집하고 결과는 컴파일 시 `Combined` `GameplayTagContainer`에 표시됩니다. `Added` 태그는 이 `GameplayEffect`가 추가하는 새로운 태그로 부모가 이전에 가지고 있지 않았던 것입니다. `Removed` 태그는 부모 클래스가 가지고 있지만 이 서브클래스가 가지고 있지 않은 태그입니다.

| 카테고리                        | 설명                                                                                                                                                                                                                                                                                                     |
| ------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 게임플레이 이펙트 에셋 태그     | `GameplayEffect`가 가진 태그입니다. 이들은 자체적으로 어떤 기능도 수행하지 않으며 `GameplayEffect`를 설명하는 목적으로만 사용됩니다.                                                                                                                                                                      |
| 부여된 태그                     | `GameplayEffect`에 존재하지만 `GameplayEffect`가 적용된 `ASC`에도 주어지는 태그입니다. `GameplayEffect`가 제거되면 `ASC`에서 제거됩니다. 이는 `Duration`과 `Infinite` `GameplayEffects`에서만 작동합니다.                                                                                                |
| 진행 중 태그 요구 사항          | 적용된 후, 이 태그들은 `GameplayEffect`가 켜져 있는지 꺼져 있는지를 결정합니다. `GameplayEffect`는 꺼져 있어도 여전히 적용될 수 있습니다. `GameplayEffect`가 진행 중 태그 요구 사항을 충족하지 못해 꺼져 있다가 요구 사항이 충족되면 다시 켜지고 수정자를 다시 적용합니다. 이는 `Duration`과 `Infinite` `GameplayEffects`에서만 작동합니다. |
| 적용 태그 요구 사항             | `GameplayEffect`가 대상에 적용될 수 있는지를 결정하는 대상의 태그입니다. 이 요구 사항이 충족되지 않으면 `GameplayEffect`는 적용되지 않습니다.                                                                                                                                                             |
| 태그가 있는 게임플레이 이펙트 제거 | 이 `GameplayEffect`가 성공적으로 적용될 때 대상에서 `Asset Tags` 또는 `Granted Tags`에 이러한 태그를 가진 `GameplayEffects`가 제거됩니다.                                                                                                                                                                |

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-ge-immunity"></a>
#### 4.5.8 면역
`GameplayEffects`는 [`GameplayTags`](#concepts-gt)를 기반으로 면역을 부여할 수 있어, 다른 `GameplayEffects`의 적용을 효과적으로 차단할 수 있습니다. 면역은 `Application Tag Requirements`와 같은 다른 방법으로도 효과적으로 달성할 수 있지만, 이 시스템을 사용하면 면역으로 인해 `GameplayEffects`가 차단될 때 델리게이트(`UAbilitySystemComponent::OnImmunityBlockGameplayEffectDelegate`)를 제공합니다.

`GrantedApplicationImmunityTags`는 소스 `ASC`(능력이 있었다면 소스 능력의 `AbilityTags`의 태그 포함)가 지정된 태그 중 하나라도 가지고 있는지 확인합니다. 이는 태그를 기반으로 특정 캐릭터나 소스의 모든 `GameplayEffects`로부터 면역을 제공하는 방법입니다.

`Granted Application Immunity Query`는 들어오는 `GameplayEffectSpec`이 쿼리와 일치하는지 확인하여 적용을 차단하거나 허용합니다.

쿼리에는 `GameplayEffect` 블루프린트에서 도움이 되는 호버 툴팁이 있습니다.

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-ge-spec"></a>
#### 4.5.9 게임플레이 이펙트 스펙
[`GameplayEffectSpec`](https://docs.unrealengine.com/en-US/API/Plugins/GameplayAbilities/FGameplayEffectSpec/index.html) (`GESpec`)은 `GameplayEffects`의 인스턴스화로 생각할 수 있습니다. 이들은 자신이 나타내는 `GameplayEffect` 클래스에 대한 참조, 생성된 레벨, 누가 생성했는지를 보유합니다. 런타임 전에 디자이너가 생성해야 하는 `GameplayEffects`와 달리, 이들은 런타임에 자유롭게 생성되고 수정될 수 있습니다. `GameplayEffect`를 적용할 때, `GameplayEffectSpec`이 `GameplayEffect`에서 생성되고 실제로 대상에 적용되는 것은 이것입니다.

`GameplayEffectSpecs`는 `UAbilitySystemComponent::MakeOutgoingSpec()`을 사용하여 `GameplayEffects`에서 생성되며, 이는 `BlueprintCallable`입니다. `GameplayEffectSpecs`는 즉시 적용될 필요가 없습니다. 능력에서 생성된 발사체에 `GameplayEffectSpec`을 전달하여 나중에 맞힌 대상에 적용하는 것이 일반적입니다. `GameplayEffectSpecs`가 성공적으로 적용되면 `FActiveGameplayEffect`라는 새로운 구조체를 반환합니다.

주목할 만한 `GameplayEffectSpec` 내용:
* 이 `GameplayEffect`가 생성된 `GameplayEffect` 클래스.
* 이 `GameplayEffectSpec`의 레벨. 일반적으로 `GameplayEffectSpec`을 생성한 능력의 레벨과 동일하지만 다를 수 있습니다.
* `GameplayEffectSpec`의 지속 시간. 기본값은 `GameplayEffect`의 지속 시간이지만 다를 수 있습니다.
* 주기적 효과를 위한 `GameplayEffectSpec`의 주기. 기본값은 `GameplayEffect`의 주기이지만 다를 수 있습니다.
* 이 `GameplayEffectSpec`의 현재 스택 카운트. 스택 제한은 `GameplayEffect`에 있습니다.
* [`GameplayEffectContextHandle`](#concepts-ge-context)은 누가 이 `GameplayEffectSpec`을 생성했는지 알려줍니다.
* 스냅샷으로 인해 `GameplayEffectSpec` 생성 시점에 캡처된 `Attributes`.
* `GameplayEffect`가 부여하는 `GameplayTags` 외에 `GameplayEffectSpec`이 대상에 부여하는 `DynamicGrantedTags`.
* `GameplayEffect`가 가진 `AssetTags` 외에 `GameplayEffectSpec`이 가진 `DynamicAssetTags`.
* `SetByCaller` `TMaps`.

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-ge-spec-setbycaller"></a>
##### 4.5.9.1 SetByCallers
`SetByCallers`는 `GameplayEffectSpec`이 `GameplayTag` 또는 `FName`과 연관된 float 값을 전달할 수 있게 합니다. 이들은 각각의 `TMaps`에 저장됩니다: `TMap<FGameplayTag, float>`와 `TMap<FName, float>`이 `GameplayEffectSpec`에 있습니다. 이들은 `GameplayEffect`의 `Modifiers`로 사용되거나 float를 전달하는 일반적인 수단으로 사용될 수 있습니다. 능력 내부에서 생성된 수치 데이터를 [`GameplayEffectExecutionCalculations`](#concepts-ge-ec)나 [`ModifierMagnitudeCalculations`](#concepts-ge-mmc)에 `SetByCallers`를 통해 전달하는 것이 일반적입니다.

| `SetByCaller` 사용 | 참고                                                                                                                                                                                                                                                                                                                                                                                |
| ------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Modifiers`         | `GameplayEffect` 클래스에서 미리 정의되어야 합니다. `GameplayTag` 버전만 사용할 수 있습니다. `GameplayEffect` 클래스에 정의되어 있지만 `GameplayEffectSpec`에 해당 태그와 float 값 쌍이 없는 경우, `GameplayEffectSpec` 적용 시 게임에서 런타임 오류가 발생하고 0을 반환합니다. 이는 `Divide` 연산에서 잠재적인 문제가 될 수 있습니다. [`Modifiers`](#concepts-ge-mods)를 참조하세요. |
| 다른 곳             | 어디에서도 미리 정의할 필요가 없습니다. `GameplayEffectSpec`에 존재하지 않는 `SetByCaller`를 읽을 때 개발자가 정의한 기본값을 선택적 경고와 함께 반환할 수 있습니다.                                                                                                                                                                                                               |

블루프린트에서 `SetByCaller` 값을 할당하려면 필요한 버전(`GameplayTag` 또는 `FName`)의 블루프린트 노드를 사용하세요:

![SetByCaller 할당](https://github.com/tranek/GASDocumentation/raw/master/Images/setbycaller.png)

블루프린트에서 `SetByCaller` 값을 읽으려면 블루프린트 라이브러리에 사용자 정의 노드를 만들어야 합니다.

C++에서 `SetByCaller` 값을 할당하려면 필요한 버전의 함수(`GameplayTag` 또는 `FName`)를 사용하세요:

```c++
void FGameplayEffectSpec::SetSetByCallerMagnitude(FName DataName, float Magnitude);
```
```c++
void FGameplayEffectSpec::SetSetByCallerMagnitude(FGameplayTag DataTag, float Magnitude);
```

C++에서 `SetByCaller` 값을 읽으려면 필요한 버전의 함수(`GameplayTag` 또는 `FName`)를 사용하세요:

```c++
float GetSetByCallerMagnitude(FName DataName, bool WarnIfNotFound = true, float DefaultIfNotFound = 0.f) const;
```
```c++
float GetSetByCallerMagnitude(FGameplayTag DataTag, bool WarnIfNotFound = true, float DefaultIfNotFound = 0.f) const;
```

`FName` 버전보다 `GameplayTag` 버전을 사용하는 것을 추천합니다. 이는 블루프린트에서 철자 오류를 방지할 수 있습니다.

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-ge-context"></a>
#### 4.5.10 게임플레이 이펙트 컨텍스트
[`GameplayEffectContext`](https://docs.unrealengine.com/en-US/API/Plugins/GameplayAbilities/FGameplayEffectContext/index.html) 구조체는 `GameplayEffectSpec`의 시작자와 [`TargetData`](#concepts-targeting-data)에 대한 정보를 보유합니다. 이는 또한 [`ModifierMagnitudeCalculations`](#concepts-ge-mmc) / [`GameplayEffectExecutionCalculations`](#concepts-ge-ec), [`AttributeSets`](#concepts-as), [`GameplayCues`](#concepts-gc)와 같은 곳 사이에서 임의의 데이터를 전달하기 위해 서브클래스화하기 좋은 구조체입니다.

`GameplayEffectContext`를 서브클래스화하려면:

1. `FGameplayEffectContext`를 서브클래스화합니다.
2. `FGameplayEffectContext::GetScriptStruct()`를 오버라이드합니다.
3. `FGameplayEffectContext::Duplicate()`를 오버라이드합니다.
4. 새로운 데이터가 복제되어야 하는 경우 `FGameplayEffectContext::NetSerialize()`를 오버라이드합니다.
5. 부모 구조체 `FGameplayEffectContext`와 같이 서브클래스에 대한 `TStructOpsTypeTraits`를 구현합니다.
6. [`AbilitySystemGlobals`](#concepts-asg) 클래스에서 `AllocGameplayEffectContext()`를 오버라이드하여 서브클래스의 새 객체를 반환하도록 합니다.

[GASShooter](https://github.com/tranek/GASShooter)는 서브클래스화된 `GameplayEffectContext`를 사용하여 `GameplayCues`에서 접근할 수 있는 `TargetData`를 추가합니다. 특히 샷건의 경우 여러 적을 맞출 수 있기 때문에 이를 사용합니다.

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-ge-mmc"></a>
#### 4.5.11 수정자 크기 계산
[`ModifierMagnitudeCalculations`](https://docs.unrealengine.com/en-US/API/Plugins/GameplayAbilities/UGameplayModMagnitudeCalculation/index.html) (`ModMagCalc` 또는 `MMC`)는 `GameplayEffects`에서 [`Modifiers`](#concepts-ge-mods)로 사용되는 강력한 클래스입니다. 이들은 [`GameplayEffectExecutionCalculations`](#concepts-ge-ec)와 유사하게 작동하지만 덜 강력하며 가장 중요한 점은 [예측](#concepts-p)될 수 있다는 것입니다. 이들의 유일한 목적은 `CalculateBaseMagnitude_Implementation()`에서 float 값을 반환하는 것입니다. 블루프린트와 C++에서 이 함수를 서브클래스화하고 오버라이드할 수 있습니다.

`MMC`는 모든 지속 시간의 `GameplayEffects`에서 사용될 수 있습니다 - `Instant`, `Duration`, `Infinite`, 또는 `Periodic`.

`MMC`의 강점은 `GameplayEffect`의 `Source` 또는 `Target`에서 임의의 수의 `Attributes` 값을 캡처할 수 있고 `GameplayTags`와 `SetByCallers`를 읽기 위해 `GameplayEffectSpec`에 완전히 접근할 수 있다는 점입니다. `Attributes`는 스냅샷되거나 스냅샷되지 않을 수 있습니다. 스냅샷된 `Attributes`는 `GameplayEffectSpec`이 생성될 때 캡처되는 반면, 스냅샷되지 않은 `Attributes`는 `GameplayEffectSpec`이 적용될 때 캡처되며 `Infinite`와 `Duration` `GameplayEffects`에 대해 `Attribute`가 변경될 때 자동으로 업데이트됩니다. `Attributes`를 캡처하면 `ASC`의 기존 mods에서 `CurrentValue`를 재계산합니다. 이 재계산은 `AbilitySet`에서 [`PreAttributeChange()`](#concepts-as-preattributechange)를 실행하지 **않으므로** 여기서 다시 클램핑을 수행해야 합니다.

| 스냅샷 | 소스 또는 대상 | `GameplayEffectSpec` 캡처 시점 | `Infinite` 또는 `Duration` `GE`에 대해 `Attribute` 변경 시 자동 업데이트 |
| ------ | --------------- | ------------------------------- | ----------------------------------------------------------------------- |
| 예     | 소스            | 생성                            | 아니오                                                                  |
| 예     | 대상            | 적용                            | 아니오                                                                  |
| 아니오 | 소스            | 적용                            | 예                                                                      |
| 아니오 | 대상            | 적용                            | 예                                                                      |

`MMC`에서 나온 결과 float는 `GameplayEffect`의 `Modifier`에서 계수와 계수 전후 추가로 더 수정될 수 있습니다.

`Target`의 마나 `Attribute`를 캡처하고 `Target`이 가진 마나의 양과 `Target`이 가질 수 있는 태그에 따라 감소량이 변하는 독 효과에서 이를 감소시키는 `MMC` 예시:
```c++
UPAMMC_PoisonMana::UPAMMC_PoisonMana()
{
	//ManaDef defined in header FGameplayEffectAttributeCaptureDefinition ManaDef;
	ManaDef.AttributeToCapture = UPAAttributeSetBase::GetManaAttribute();
	ManaDef.AttributeSource = EGameplayEffectAttributeCaptureSource::Target;
	ManaDef.bSnapshot = false;

	//MaxManaDef defined in header FGameplayEffectAttributeCaptureDefinition MaxManaDef;
	MaxManaDef.AttributeToCapture = UPAAttributeSetBase::GetMaxManaAttribute();
	MaxManaDef.AttributeSource = EGameplayEffectAttributeCaptureSource::Target;
	MaxManaDef.bSnapshot = false;

	RelevantAttributesToCapture.Add(ManaDef);
	RelevantAttributesToCapture.Add(MaxManaDef);
}

float UPAMMC_PoisonMana::CalculateBaseMagnitude_Implementation(const FGameplayEffectSpec & Spec) const
{
	// 소스와 대상의 태그를 수집합니다. 이는 어떤 버프를 사용해야 하는지에 영향을 줄 수 있습니다.
	const FGameplayTagContainer* SourceTags = Spec.CapturedSourceTags.GetAggregatedTags();
	const FGameplayTagContainer* TargetTags = Spec.CapturedTargetTags.GetAggregatedTags();

	FAggregatorEvaluateParameters EvaluationParameters;
	EvaluationParameters.SourceTags = SourceTags;
	EvaluationParameters.TargetTags = TargetTags;

	float Mana = 0.f;
	GetCapturedAttributeMagnitude(ManaDef, Spec, EvaluationParameters, Mana);
	Mana = FMath::Max<float>(Mana, 0.0f);

	float MaxMana = 0.f;
	GetCapturedAttributeMagnitude(MaxManaDef, Spec, EvaluationParameters, MaxMana);
	MaxMana = FMath::Max<float>(MaxMana, 1.0f); // 0으로 나누기 방지

	float Reduction = -20.0f;
	if (Mana / MaxMana > 0.5f)
	{
		// 대상이 마나의 절반 이상을 가지고 있으면 효과를 두 배로 합니다
		Reduction *= 2;
	}
	
	if (TargetTags->HasTagExact(FGameplayTag::RequestGameplayTag(FName("Status.WeakToPoisonMana"))))
	{
		// 대상이 PoisonMana에 약하면 효과를 두 배로 합니다
		Reduction *= 2;
	}
	
	return Reduction;
}
```

`MMC`의 생성자에서 `FGameplayEffectAttributeCaptureDefinition`을 `RelevantAttributesToCapture`에 추가하지 않고 `Attributes`를 캡처하려고 하면 캡처 중 누락된 Spec에 대한 오류가 발생합니다. `Attributes`를 캡처할 필요가 없다면 `RelevantAttributesToCapture`에 아무것도 추가할 필요가 없습니다.

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-ge-ec"></a>
#### 4.5.12 게임플레이 이펙트 실행 계산
[`GameplayEffectExecutionCalculations`](https://docs.unrealengine.com/en-US/API/Plugins/GameplayAbilities/UGameplayEffectExecutionCalculat-/index.html) (`ExecutionCalculation`, `Execution` (플러그인 소스 코드에서 자주 볼 수 있는 용어), 또는 `ExecCalc`)는 `GameplayEffects`가 `ASC`에 변경을 가하는 가장 강력한 방법입니다. [`ModifierMagnitudeCalculations`](#concepts-ge-mmc)와 마찬가지로, 이들은 `Attributes`를 캡처하고 선택적으로 스냅샷할 수 있습니다. `MMC`와 달리, 이들은 하나 이상의 `Attribute`를 변경할 수 있고 본질적으로 프로그래머가 원하는 다른 모든 것을 할 수 있습니다. 이러한 힘과 유연성의 단점은 [예측](#concepts-p)할 수 없다는 것과 C++로 구현해야 한다는 것입니다.

`ExecutionCalculations`는 `Instant`와 `Periodic` `GameplayEffects`에서만 사용할 수 있습니다. 'Execute'라는 단어가 포함된 모든 것은 일반적으로 이 두 가지 유형의 `GameplayEffects`를 참조합니다.

스냅샷팅은 `GameplayEffectSpec`이 생성될 때 `Attribute`를 캡처하는 반면, 스냅샷팅을 하지 않으면 `GameplayEffectSpec`이 적용될 때 `Attribute`를 캡처합니다. `Attributes`를 캡처하면 `ASC`의 기존 mods에서 `CurrentValue`를 재계산합니다. 이 재계산은 `AbilitySet`에서 [`PreAttributeChange()`](#concepts-as-preattributechange)를 실행하지 **않으므로** 여기서 다시 클램핑을 수행해야 합니다.

| 스냅샷 | 소스 또는 대상 | `GameplayEffectSpec` 캡처 시점 |
| ------ | --------------- | ------------------------------- |
| 예     | 소스            | 생성                            |
| 예     | 대상            | 적용                            |
| 아니오 | 소스            | 적용                            |
| 아니오 | 대상            | 적용                            |

`Attribute` 캡처를 설정하기 위해, Epic의 ActionRPG 샘플 프로젝트에서 설정한 패턴을 따라 `Attributes`를 캡처하는 방법을 정의하고 보유하는 구조체를 정의하고 구조체의 생성자에서 하나의 복사본을 생성합니다. 각 `ExecCalc`에 대해 이와 같은 구조체가 있을 것입니다. **참고:** 각 구조체는 동일한 네임스페이스를 공유하므로 고유한 이름이 필요합니다. 구조체에 동일한 이름을 사용하면 `Attributes` 캡처에 잘못된 동작이 발생합니다(주로 잘못된 `Attributes`의 값을 캡처함).

`Local Predicted`, `Server Only`, `Server Initiated` [`GameplayAbilities`](#concepts-ga)의 경우, `ExecCalc`는 서버에서만 호출됩니다.

`Source`와 `Target`의 많은 어트리뷰트에서 읽는 복잡한 공식을 기반으로 받은 데미지를 계산하는 것이 `ExecCalc`의 가장 일반적인 예입니다. 포함된 샘플 프로젝트에는 `GameplayEffectSpec`의 [`SetByCaller`](#concepts-ge-spec-setbycaller)에서 데미지 값을 읽고 `Target`에서 캡처한 방어구 `Attribute`를 기반으로 해당 값을 완화하는 간단한 데미지 계산 `ExecCalc`가 있습니다. `GDDamageExecCalculation.cpp/.h`를 참조하세요.

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-ge-ec-senddata"></a>
##### 4.5.12.1 실행 계산에 데이터 보내기
`Attributes`를 캡처하는 것 외에도 `ExecutionCalculation`에 데이터를 보내는 몇 가지 방법이 있습니다.

<a name="concepts-ge-ec-senddata-setbycaller"></a>
###### 4.5.12.1.1 SetByCaller
[`GameplayEffectSpec`에 설정된 `SetByCallers`](#concepts-ge-spec-setbycaller)는 `ExecutionCalculation`에서 직접 읽을 수 있습니다.

```c++
const FGameplayEffectSpec& Spec = ExecutionParams.GetOwningSpec();
float Damage = FMath::Max<float>(Spec.GetSetByCallerMagnitude(FGameplayTag::RequestGameplayTag(FName("Data.Damage")), false, -1.0f), 0.0f);
```

<a name="concepts-ge-ec-senddata-backingdataattribute"></a>
###### 4.5.12.1.2 백업 데이터 어트리뷰트 계산 수정자
`GameplayEffect`에 값을 하드코딩하려면 캡처된 `Attributes` 중 하나를 백업 데이터로 사용하는 `CalculationModifier`를 사용하여 전달할 수 있습니다.

이 스크린샷 예시에서는 캡처된 Damage `Attribute`에 50을 추가하고 있습니다. 이를 `Override`로 설정하여 하드코딩된 값만 사용할 수도 있습니다.

![백업 데이터 어트리뷰트 계산 수정자](https://github.com/tranek/GASDocumentation/raw/master/Images/calculationmodifierbackingdataattribute.png)

`ExecutionCalculation`은 `Attribute`를 캡처할 때 이 값을 읽습니다.

```c++
float Damage = 0.0f;
// ExecutionCalculation 아래의 CalculationModifier로 데미지 GE에 설정된 선택적 데미지 값 캡처
ExecutionParams.AttemptCalculateCapturedAttributeMagnitude(DamageStatics().DamageDef, EvaluationParameters, Damage);
```

<a name="concepts-ge-ec-senddata-backingdatatempvariable"></a>
###### 4.5.12.1.3 백업 데이터 임시 변수 계산 수정자
`GameplayEffect`에 값을 하드코딩하려면 `Temporary Variable` 또는 C++에서 `Transient Aggregator`라고 불리는 것을 사용하는 `CalculationModifier`를 사용하여 전달할 수 있습니다. `Temporary Variable`은 `GameplayTag`와 연관됩니다.

이 스크린샷 예시에서는 `Data.Damage` `GameplayTag`를 사용하여 `Temporary Variable`에 50을 추가하고 있습니다.

![백업 데이터 임시 변수 계산 수정자](https://github.com/tranek/GASDocumentation/raw/master/Images/calculationmodifierbackingdatatempvariable.png)

`ExecutionCalculation`의 생성자에 백업 `Temporary Variables`를 추가합니다:

```c++
ValidTransientAggregatorIdentifiers.AddTag(FGameplayTag::RequestGameplayTag("Data.Damage"));
```

`ExecutionCalculation`은 `Attribute` 캡처 함수와 유사한 특별한 캡처 함수를 사용하여 이 값을 읽습니다.

```c++
float Damage = 0.0f;
ExecutionParams.AttemptCalculateTransientAggregatorMagnitude(FGameplayTag::RequestGameplayTag("Data.Damage"), EvaluationParameters, Damage);
```

<a name="concepts-ge-ec-senddata-effectcontext"></a>
###### 4.5.12.1.4 게임플레이 이펙트 컨텍스트
[`GameplayEffectSpec`의 사용자 정의 `GameplayEffectContext`](#concepts-ge-context)를 통해 `ExecutionCalculation`에 데이터를 보낼 수 있습니다.

`ExecutionCalculation`에서 `FGameplayEffectCustomExecutionParameters`로부터 `EffectContext`에 접근할 수 있습니다.

```c++
const FGameplayEffectSpec& Spec = ExecutionParams.GetOwningSpec();
FGSGameplayEffectContext* ContextHandle = static_cast<FGSGameplayEffectContext*>(Spec.GetContext().Get());
```

`GameplayEffectSpec` 또는 `EffectContext`에서 무언가를 변경해야 하는 경우:

```c++
FGameplayEffectSpec* MutableSpec = ExecutionParams.GetOwningSpecForPreExecuteMod();
FGSGameplayEffectContext* ContextHandle = static_cast<FGSGameplayEffectContext*>(MutableSpec->GetContext().Get());
```

`ExecutionCalculation`에서 `GameplayEffectSpec`을 수정할 때는 주의해야 합니다. `GetOwningSpecForPreExecuteMod()`의 주석을 참조하세요.

```c++
/** Non const access. Be careful with this, especially when modifying a spec after attribute capture. */
FGameplayEffectSpec* GetOwningSpecForPreExecuteMod() const;
```

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-ge-car"></a>
#### 4.5.13 사용자 정의 적용 요구사항
[`CustomApplicationRequirement`](https://docs.unrealengine.com/en-US/API/Plugins/GameplayAbilities/UGameplayEffectCustomApplication-/index.html) (`CAR`) 클래스는 디자이너에게 `GameplayEffect`의 단순한 `GameplayTag` 검사보다 `GameplayEffect`를 적용할 수 있는지 여부에 대한 고급 제어 기능을 제공합니다. 이는 블루프린트에서 `CanApplyGameplayEffect()`를 오버라이드하거나 C++에서 `CanApplyGameplayEffect_Implementation()`을 오버라이드하여 구현할 수 있습니다.

`CAR`을 사용하는 경우의 예:
* `Target`이 특정 양의 `Attribute`를 가져야 하는 경우
* `Target`이 `GameplayEffect`의 특정 수의 스택을 가져야 하는 경우

`CAR`은 이 `GameplayEffect`의 인스턴스가 이미 `Target`에 있는지 확인하고 새 인스턴스를 적용하는 대신 기존 인스턴스의 [지속 시간을 변경](#concepts-ge-duration)하는 등의 더 고급 작업을 수행할 수도 있습니다(`CanApplyGameplayEffect()`에 대해 false를 반환).

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-ge-cost"></a>
#### 4.5.14 비용 게임플레이 이펙트
[`GameplayAbilities`](#concepts-ga)에는 능력의 비용으로 사용하도록 특별히 설계된 선택적 `GameplayEffect`가 있습니다. 비용은 `ASC`가 `GameplayAbility`를 활성화하기 위해 필요한 `Attribute`의 양입니다. `GA`가 `Cost GE`를 감당할 수 없으면 활성화할 수 없습니다. 이 `Cost GE`는 `Attributes`에서 차감하는 하나 이상의 `Modifiers`가 있는 `Instant` `GameplayEffect`여야 합니다. 기본적으로 `Cost GE`는 예측되도록 되어 있으며, 이 기능을 유지하는 것이 권장됩니다. 즉, `ExecutionCalculations`를 사용하지 않아야 합니다. 복잡한 비용 계산을 위해 `MMC`를 사용하는 것은 완전히 허용되며 권장됩니다.

시작할 때는 비용이 있는 `GA`마다 고유한 `Cost GE`를 하나씩 가질 가능성이 높습니다. 더 고급 기술은 여러 `GA`에 대해 하나의 `Cost GE`를 재사용하고 `Cost GE`에서 생성된 `GameplayEffectSpec`을 `GA` 특정 데이터로 수정하는 것입니다(비용 값은 `GA`에 정의됨). **이는 `Instanced` 능력에만 작동합니다.**

`Cost GE`를 재사용하는 두 가지 기술:

1. **`MMC` 사용.** 이는 가장 쉬운 방법입니다. `GameplayEffectSpec`에서 얻을 수 있는 `GameplayAbility` 인스턴스에서 비용 값을 읽는 [`MMC`](#concepts-ge-mmc)를 생성합니다.

```c++
float UPGMMC_HeroAbilityCost::CalculateBaseMagnitude_Implementation(const FGameplayEffectSpec & Spec) const
{
	const UPGGameplayAbility* Ability = Cast<UPGGameplayAbility>(Spec.GetContext().GetAbilityInstance_NotReplicated());

	if (!Ability)
	{
		return 0.0f;
	}

	return Ability->Cost.GetValueAtLevel(Ability->GetAbilityLevel());
}
```

이 예에서 비용 값은 `GameplayAbility` 자식 클래스에 추가한 `FScalableFloat`입니다.
```c++
UPROPERTY(BlueprintReadOnly, EditAnywhere, Category = "Cost")
FScalableFloat Cost;
```

![MMC가 있는 Cost GE](https://github.com/tranek/GASDocumentation/raw/master/Images/costmmc.png)

2. **`UGameplayAbility::GetCostGameplayEffect()` 오버라이드.** 이 함수를 오버라이드하고 `GameplayAbility`의 비용 값을 읽는 [런타임에 `GameplayEffect` 생성](#concepts-ge-dynamic)합니다.

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-ge-cooldown"></a>
#### 4.5.15 쿨다운 게임플레이 이펙트
[`GameplayAbilities`](#concepts-ga)는 능력의 쿨다운으로 사용하도록 특별히 설계된 선택적 `GameplayEffect`를 가집니다. 쿨다운은 능력 활성화 후 다시 활성화할 수 있는 시간을 결정합니다. `GA`가 여전히 쿨다운 중이라면 활성화할 수 없습니다. 이 `Cooldown GE`는 `Modifiers`가 없고 `GameplayEffect`의 `GrantedTags`("쿨다운 태그")에 `GameplayAbility`당 또는 능력 슬롯당(게임에 쿨다운을 공유하는 슬롯에 할당된 교체 가능한 능력이 있는 경우) 고유한 `GameplayTag`가 있는 `Duration` `GameplayEffect`여야 합니다. `GA`는 실제로 `Cooldown GE`의 존재 대신 `Cooldown Tag`의 존재를 확인합니다. 기본적으로 `Cooldown GEs`는 예측되도록 되어 있으며, 이 기능을 유지하는 것이 권장됩니다. 즉, `ExecutionCalculations`를 사용하지 않아야 합니다. 복잡한 쿨다운 계산을 위해 `MMC`를 사용하는 것은 완전히 허용되며 권장됩니다.

시작할 때는 쿨다운이 있는 `GA`마다 고유한 `Cooldown GE`를 하나씩 가질 가능성이 높습니다. 더 고급 기술은 여러 `GA`에 대해 하나의 `Cooldown GE`를 재사용하고 `Cooldown GE`에서 생성된 `GameplayEffectSpec`을 `GA` 특정 데이터로 수정하는 것입니다(쿨다운 지속 시간과 `Cooldown Tag`는 `GA`에 정의됨). **이는 `Instanced` 능력에만 작동합니다.**

`Cooldown GE`를 재사용하는 두 가지 기술:

1. **[`SetByCaller`](#concepts-ge-spec-setbycaller) 사용.** 이는 가장 쉬운 방법입니다. 공유 `Cooldown GE`의 지속 시간을 `GameplayTag`와 함께 `SetByCaller`로 설정합니다. `GameplayAbility` 서브클래스에서 지속 시간에 대한 float / `FScalableFloat`, 고유한 `Cooldown Tag`에 대한 `FGameplayTagContainer`, 그리고 `Cooldown Tag`와 `Cooldown GE`의 태그의 합집합의 반환 포인터로 사용할 임시 `FGameplayTagContainer`를 정의합니다.
```c++
UPROPERTY(BlueprintReadOnly, EditAnywhere, Category = "Cooldown")
FScalableFloat CooldownDuration;

UPROPERTY(BlueprintReadOnly, EditAnywhere, Category = "Cooldown")
FGameplayTagContainer CooldownTags;

// GetCooldownTags()에서 포인터를 반환할 임시 컨테이너입니다.
// 이는 우리의 CooldownTags와 Cooldown GE의 쿨다운 태그의 합집합이 될 것입니다.
UPROPERTY(Transient)
FGameplayTagContainer TempCooldownTags;
```

그런 다음 `UGameplayAbility::GetCooldownTags()`를 오버라이드하여 `Cooldown Tags`와 기존 `Cooldown GE`의 태그의 합집합을 반환합니다.
```c++
const FGameplayTagContainer * UPGGameplayAbility::GetCooldownTags() const
{
	FGameplayTagContainer* MutableTags = const_cast<FGameplayTagContainer*>(&TempCooldownTags);
	MutableTags->Reset(); // MutableTags는 CDO의 TempCooldownTags에 쓰므로 능력 쿨다운 태그가 변경된 경우(다른 슬롯으로 이동) 이를 지웁니다
	const FGameplayTagContainer* ParentTags = Super::GetCooldownTags();
	if (ParentTags)
	{
		MutableTags->AppendTags(*ParentTags);
	}
	MutableTags->AppendTags(CooldownTags);
	return MutableTags;
}
```

마지막으로, `UGameplayAbility::ApplyCooldown()`을 오버라이드하여 `Cooldown Tags`를 주입하고 쿨다운 `GameplayEffectSpec`에 `SetByCaller`를 추가합니다.
```c++
void UPGGameplayAbility::ApplyCooldown(const FGameplayAbilitySpecHandle Handle, const FGameplayAbilityActorInfo * ActorInfo, const FGameplayAbilityActivationInfo ActivationInfo) const
{
	UGameplayEffect* CooldownGE = GetCooldownGameplayEffect();
	if (CooldownGE)
	{
		FGameplayEffectSpecHandle SpecHandle = MakeOutgoingGameplayEffectSpec(CooldownGE->GetClass(), GetAbilityLevel());
		SpecHandle.Data.Get()->DynamicGrantedTags.AppendTags(CooldownTags);
		SpecHandle.Data.Get()->SetSetByCallerMagnitude(FGameplayTag::RequestGameplayTag(FName(  OurSetByCallerTag  )), CooldownDuration.GetValueAtLevel(GetAbilityLevel()));
		ApplyGameplayEffectSpecToOwner(Handle, ActorInfo, ActivationInfo, SpecHandle);
	}
}
```

이 그림에서 쿨다운의 지속 시간 `Modifier`는 `Data.Cooldown`이라는 `Data Tag`와 함께 `SetByCaller`로 설정됩니다. `Data.Cooldown`은 위 코드에서 `OurSetByCallerTag`가 될 것입니다.

![SetByCaller가 있는 Cooldown GE](https://github.com/tranek/GASDocumentation/raw/master/Images/cooldownsbc.png)

2. **[`MMC`](#concepts-ge-mmc) 사용.** 이는 `Cooldown GE`와 `ApplyCooldown`에서 지속 시간을 `SetByCaller`로 설정하는 것을 제외하고 위와 동일한 설정입니다. 대신, 지속 시간을 `Custom Calculation Class`로 설정하고 우리가 만들 새로운 `MMC`를 가리킵니다.
```c++
UPROPERTY(BlueprintReadOnly, EditAnywhere, Category = "Cooldown")
FScalableFloat CooldownDuration;

UPROPERTY(BlueprintReadOnly, EditAnywhere, Category = "Cooldown")
FGameplayTagContainer CooldownTags;

// GetCooldownTags()에서 포인터를 반환할 임시 컨테이너입니다.
// 이는 우리의 CooldownTags와 Cooldown GE의 쿨다운 태그의 합집합이 될 것입니다.
UPROPERTY(Transient)
FGameplayTagContainer TempCooldownTags;
```

그런 다음 `UGameplayAbility::GetCooldownTags()`를 오버라이드하여 `Cooldown Tags`와 기존 `Cooldown GE`의 태그의 합집합을 반환합니다.
```c++
const FGameplayTagContainer * UPGGameplayAbility::GetCooldownTags() const
{
	FGameplayTagContainer* MutableTags = const_cast<FGameplayTagContainer*>(&TempCooldownTags);
	MutableTags->Reset(); // MutableTags는 CDO의 TempCooldownTags에 쓰므로 능력 쿨다운 태그가 변경된 경우(다른 슬롯으로 이동) 이를 지웁니다
	const FGameplayTagContainer* ParentTags = Super::GetCooldownTags();
	if (ParentTags)
	{
		MutableTags->AppendTags(*ParentTags);
	}
	MutableTags->AppendTags(CooldownTags);
	return MutableTags;
}
```

마지막으로, `UGameplayAbility::ApplyCooldown()`을 오버라이드하여 `Cooldown Tags`를 쿨다운 `GameplayEffectSpec`에 주입합니다.
```c++
void UPGGameplayAbility::ApplyCooldown(const FGameplayAbilitySpecHandle Handle, const FGameplayAbilityActorInfo * ActorInfo, const FGameplayAbilityActivationInfo ActivationInfo) const
{
	UGameplayEffect* CooldownGE = GetCooldownGameplayEffect();
	if (CooldownGE)
	{
		FGameplayEffectSpecHandle SpecHandle = MakeOutgoingGameplayEffectSpec(CooldownGE->GetClass(), GetAbilityLevel());
		SpecHandle.Data.Get()->DynamicGrantedTags.AppendTags(CooldownTags);
		ApplyGameplayEffectSpecToOwner(Handle, ActorInfo, ActivationInfo, SpecHandle);
	}
}
```

`MMC` 생성:
```c++
float UPGMMC_HeroAbilityCooldown::CalculateBaseMagnitude_Implementation(const FGameplayEffectSpec & Spec) const
{
	const UPGGameplayAbility* Ability = Cast<UPGGameplayAbility>(Spec.GetContext().GetAbilityInstance_NotReplicated());

	if (!Ability)
	{
		return 0.0f;
	}

	return Ability->CooldownDuration.GetValueAtLevel(Ability->GetAbilityLevel());
}
```

이 그림에서 쿨다운의 지속 시간 `Modifier`는 새로운 `MMC`를 가리키는 `Custom Calculation Class`로 설정됩니다.

![MMC가 있는 Cooldown GE](https://github.com/tranek/GASDocumentation/raw/master/Images/cooldownmmc.png)

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-ge-cooldown-tr"></a>
##### 4.5.15.1 쿨다운 게임플레이 이펙트의 남은 시간 가져오기
```c++
bool APGPlayerState::GetCooldownRemainingForTag(FGameplayTagContainer CooldownTags, float & TimeRemaining, float & CooldownDuration)
{
	if (AbilitySystemComponent && CooldownTags.Num() > 0)
	{
		TimeRemaining = 0.f;
		CooldownDuration = 0.f;

		FGameplayEffectQuery const Query = FGameplayEffectQuery::MakeQuery_MatchAnyOwningTags(CooldownTags);
		TArray< TPair<float, float> > DurationAndTimeRemaining = AbilitySystemComponent->GetActiveEffectsTimeRemainingAndDuration(Query);
		if (DurationAndTimeRemaining.Num() > 0)
		{
			int32 BestIdx = 0;
			float LongestTime = DurationAndTimeRemaining[0].Key;
			for (int32 Idx = 1; Idx < DurationAndTimeRemaining.Num(); ++Idx)
			{
				if (DurationAndTimeRemaining[Idx].Key > LongestTime)
				{
					LongestTime = DurationAndTimeRemaining[Idx].Key;
					BestIdx = Idx;
				}
			}

			TimeRemaining = DurationAndTimeRemaining[BestIdx].Key;
			CooldownDuration = DurationAndTimeRemaining[BestIdx].Value;

			return true;
		}
	}

	return false;
}
```

**참고:** 클라이언트에서 쿨다운의 남은 시간을 쿼리하려면 복제된 `GameplayEffects`를 받을 수 있어야 합니다. 이는 `ASC`의 [복제 모드](#concepts-asc-rm)에 따라 달라집니다.

<a name="concepts-ge-cooldown-listen"></a>
##### 4.5.15.2 쿨다운 시작과 종료 수신하기
쿨다운이 시작될 때를 수신하려면 `AbilitySystemComponent->OnActiveGameplayEffectAddedDelegateToSelf`에 바인딩하여 `Cooldown GE`가 적용될 때 응답하거나 `AbilitySystemComponent->RegisterGameplayTagEvent(CooldownTag, EGameplayTagEventType::NewOrRemoved)`에 바인딩하여 `Cooldown Tag`가 추가될 때 응답할 수 있습니다. `Cooldown GE`가 추가될 때를 수신하는 것을 추천합니다. 이렇게 하면 적용된 `GameplayEffectSpec`에도 접근할 수 있기 때문입니다. 이를 통해 `Cooldown GE`가 로컬에서 예측된 것인지 서버의 수정된 것인지 판단할 수 있습니다.

쿨다운이 종료될 때를 수신하려면 `AbilitySystemComponent->OnAnyGameplayEffectRemovedDelegate()`에 바인딩하여 `Cooldown GE`가 제거될 때 응답하거나 `AbilitySystemComponent->RegisterGameplayTagEvent(CooldownTag, EGameplayTagEventType::NewOrRemoved)`에 바인딩하여 `Cooldown Tag`가 제거될 때 응답할 수 있습니다. `Cooldown Tag`가 제거될 때를 수신하는 것을 추천합니다. 서버의 수정된 `Cooldown GE`가 들어오면 우리의 로컬에서 예측된 것을 제거하여 여전히 쿨다운 중임에도 불구하고 `OnAnyGameplayEffectRemovedDelegate()`가 발생하기 때문입니다. 예측된 `Cooldown GE`의 제거와 서버의 수정된 `Cooldown GE`의 적용 동안 `Cooldown Tag`는 변경되지 않을 것입니다.

**참고:** 클라이언트에서 `GameplayEffect`가 추가되거나 제거되는 것을 수신하려면 복제된 `GameplayEffects`를 받을 수 있어야 합니다. 이는 `ASC`의 [복제 모드](#concepts-asc-rm)에 따라 달라집니다.

샘플 프로젝트에는 쿨다운의 시작과 종료를 수신하는 사용자 정의 블루프린트 노드가 포함되어 있습니다. HUD UMG 위젯은 이를 사용하여 Meteor의 쿨다운 남은 시간을 업데이트합니다. 이 `AsyncTask`는 수동으로 `EndTask()`가 호출될 때까지 영원히 살아있을 것이며, 우리는 UMG 위젯의 `Destruct` 이벤트에서 이를 수행합니다. [`AsyncTaskCooldownChanged.h/cpp`](Source/GASDocumentation/Private/Characters/Abilities/AsyncTaskCooldownChanged.cpp)를 참조하세요.

![쿨다운 변경 수신 BP 노드](https://github.com/tranek/GASDocumentation/raw/master/Images/cooldownchange.png)

<a name="concepts-ge-cooldown-prediction"></a>
##### 4.5.15.3 쿨다운 예측
현재 쿨다운을 실제로 예측할 수는 없습니다. 로컬에서 예측된 `Cooldown GE`가 적용될 때 UI 쿨다운 타이머를 시작할 수 있지만 `GameplayAbility`의 실제 쿨다운은 서버의 쿨다운 남은 시간과 연결되어 있습니다. 플레이어의 지연 시간에 따라 로컬에서 예측된 쿨다운이 만료되었지만 서버에서는 여전히 `GameplayAbility`가 쿨다운 중일 수 있으며, 이는 서버의 쿨다운이 만료될 때까지 `GameplayAbility`의 즉시 재활성화를 방지할 것입니다.

샘플 프로젝트는 로컬에서 예측된 쿨다운이 시작될 때 Meteor 능력의 UI 아이콘을 회색으로 처리하고 서버의 수정된 `Cooldown GE`가 들어오면 쿨다운 타이머를 시작하는 방식으로 이를 처리합니다.

이의 게임플레이 결과로, 높은 지연 시간을 가진 플레이어들은 낮은 지연 시간을 가진 플레이어들보다 짧은 쿨다운 능력의 발사 속도가 낮아져 불이익을 받게 됩니다. Fortnite는 무기에 쿨다운 `GameplayEffects`를 사용하지 않는 사용자 정의 북키핑을 사용하여 이를 피합니다.

진정한 예측 쿨다운(로컬 쿨다운이 만료되었지만 서버가 여전히 쿨다운 중일 때 플레이어가 `GameplayAbility`를 활성화할 수 있음)을 허용하는 것은 Epic이 [GAS의 미래 반복](#concepts-p-future)에서 언젠가 구현하고 싶어하는 것입니다.

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-ge-duration"></a>
#### 4.5.16 활성 게임플레이 이펙트 지속 시간 변경
`Cooldown GE` 또는 모든 `Duration` `GameplayEffect`의 남은 시간을 변경하려면 `GameplayEffectSpec`의 `Duration`을 변경하고, `StartServerWorldTime`을 업데이트하고, `CachedStartServerWorldTime`을 업데이트하고, `StartWorldTime`을 업데이트한 다음 `CheckDuration()`으로 지속 시간 검사를 다시 실행해야 합니다. 서버에서 이를 수행하고 `FActiveGameplayEffect`를 더티로 표시하면 변경 사항이 클라이언트에 복제됩니다.
**참고:** 이는 `const_cast`를 포함하며 Epic이 의도한 지속 시간 변경 방법이 아닐 수 있지만, 지금까지는 잘 작동하는 것 같습니다.

```c++
bool UPAAbilitySystemComponent::SetGameplayEffectDurationHandle(FActiveGameplayEffectHandle Handle, float NewDuration)
{
	if (!Handle.IsValid())
	{
		return false;
	}

	const FActiveGameplayEffect* ActiveGameplayEffect = GetActiveGameplayEffect(Handle);
	if (!ActiveGameplayEffect)
	{
		return false;
	}

	FActiveGameplayEffect* AGE = const_cast<FActiveGameplayEffect*>(ActiveGameplayEffect);
	if (NewDuration > 0)
	{
		AGE->Spec.Duration = NewDuration;
	}
	else
	{
		AGE->Spec.Duration = 0.01f;
	}

	AGE->StartServerWorldTime = ActiveGameplayEffects.GetServerWorldTime();
	AGE->CachedStartServerWorldTime = AGE->StartServerWorldTime;
	AGE->StartWorldTime = ActiveGameplayEffects.GetWorldTime();
	ActiveGameplayEffects.MarkItemDirty(*AGE);
	ActiveGameplayEffects.CheckDuration(Handle);

	AGE->EventSet.OnTimeChanged.Broadcast(AGE->Handle, AGE->StartWorldTime, AGE->GetDuration());
	OnGameplayEffectDurationChange(*AGE);

	return true;
}
```

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-ge-dynamic"></a>
#### 4.5.17 런타임에 동적 게임플레이 이펙트 생성
런타임에 동적 `GameplayEffects`를 생성하는 것은 고급 주제입니다. 이를 너무 자주 할 필요는 없습니다.

C++에서 처음부터 런타임에 생성할 수 있는 것은 `Instant` `GameplayEffects`뿐입니다. `Duration`과 `Infinite` `GameplayEffects`는 복제될 때 존재하지 않는 `GameplayEffect` 클래스 정의를 찾기 때문에 런타임에 동적으로 생성할 수 없습니다. 이 기능을 달성하려면 대신 일반적으로 에디터에서 하는 것처럼 원형 `GameplayEffect` 클래스를 만들어야 합니다. 그런 다음 런타임에 필요한 대로 `GameplayEffectSpec` 인스턴스를 사용자 정의합니다.

런타임에 생성된 `Instant` `GameplayEffects`는 [로컬 예측](#concepts-p) `GameplayAbility` 내에서도 호출될 수 있습니다. 그러나 동적 생성이 부작용을 가질 수 있는지는 아직 알려지지 않았습니다.

##### 예시

샘플 프로젝트는 `AttributeSet`에서 캐릭터가 치명타를 입을 때 킬러에게 금과 경험치를 돌려보내기 위해 하나를 생성합니다.

```c++
// 보상을 주기 위한 동적 즉시 Gameplay Effect 생성
UGameplayEffect* GEBounty = NewObject<UGameplayEffect>(GetTransientPackage(), FName(TEXT("Bounty")));
GEBounty->DurationPolicy = EGameplayEffectDurationType::Instant;

int32 Idx = GEBounty->Modifiers.Num();
GEBounty->Modifiers.SetNum(Idx + 2);

FGameplayModifierInfo& InfoXP = GEBounty->Modifiers[Idx];
InfoXP.ModifierMagnitude = FScalableFloat(GetXPBounty());
InfoXP.ModifierOp = EGameplayModOp::Additive;
InfoXP.Attribute = UGDAttributeSetBase::GetXPAttribute();

FGameplayModifierInfo& InfoGold = GEBounty->Modifiers[Idx + 1];
InfoGold.ModifierMagnitude = FScalableFloat(GetGoldBounty());
InfoGold.ModifierOp = EGameplayModOp::Additive;
InfoGold.Attribute = UGDAttributeSetBase::GetGoldAttribute();

Source->ApplyGameplayEffectToSelf(GEBounty, 1.0f, Source->MakeEffectContext());
```

두 번째 예시는 로컬 예측 `GameplayAbility` 내에서 생성된 런타임 `GameplayEffect`를 보여줍니다. 자신의 책임 하에 사용하세요 (코드의 주석 참조)!

```c++
UGameplayAbilityRuntimeGE::UGameplayAbilityRuntimeGE()
{
	NetExecutionPolicy = EGameplayAbilityNetExecutionPolicy::LocalPredicted;
}

void UGameplayAbilityRuntimeGE::ActivateAbility(const FGameplayAbilitySpecHandle Handle, const FGameplayAbilityActorInfo* ActorInfo, const FGameplayAbilityActivationInfo ActivationInfo, const FGameplayEventData* TriggerEventData)
{
	if (HasAuthorityOrPredictionKey(ActorInfo, &ActivationInfo))
	{
		if (!CommitAbility(Handle, ActorInfo, ActivationInfo))
		{
			EndAbility(Handle, ActorInfo, ActivationInfo, true, true);
		}

		// 런타임에 GE 생성.
		UGameplayEffect* GameplayEffect = NewObject<UGameplayEffect>(GetTransientPackage(), TEXT("RuntimeInstantGE"));
		GameplayEffect->DurationPolicy = EGameplayEffectDurationType::Instant; // 런타임 GE에서는 즉시만 작동합니다.

		// MyAttribute를 42로 덮어쓰는 간단한 확장 가능한 float 수정자 추가.
		// 실제 응용 프로그램에서는 TriggerEventData를 통해 전달된 정보를 사용합니다.
		const int32 Idx = GameplayEffect->Modifiers.Num();
		GameplayEffect->Modifiers.SetNum(Idx + 1);
		FGameplayModifierInfo& ModifierInfo = GameplayEffect->Modifiers[Idx];
		ModifierInfo.Attribute.SetUProperty(UMyAttributeSet::GetMyModifiedAttribute());
		ModifierInfo.ModifierMagnitude = FScalableFloat(42.f);
		ModifierInfo.ModifierOp = EGameplayModOp::Override;

		// GE 적용.

		// ASC가 GE 클래스 기본 객체에서 GESpec을 생성하는 동작을 피하기 위해 여기서 GESpec을 생성합니다.
		// 여기에 동적 GE가 있으므로 기본 GameplayEffect 클래스로 GESpec을 생성하여 수정자를 잃게 됩니다.
		// 주의: 여기서 수행된 이 "해킹"이 단점을 가질 수 있는지는 알려지지 않았습니다!
		// 스펙은 GE가 스펙의 UPROPERTY이므로 GE 객체가 GarbageCollector에 의해 수집되는 것을 방지합니다.
		FGameplayEffectSpec* GESpec = new FGameplayEffectSpec(GameplayEffect, {}, 0.f); // "new"는 핸들 내의 공유 포인터에 의해 수명이 관리되기 때문입니다
		ApplyGameplayEffectSpecToOwner(Handle, ActorInfo, ActivationInfo, FGameplayEffectSpecHandle(GESpec));
	}
	EndAbility(Handle, ActorInfo, ActivationInfo, false, false);
}
```

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-ge-containers"></a>
#### 4.5.18 게임플레이 이펙트 컨테이너
Epic의 [Action RPG 샘플 프로젝트](https://www.unrealengine.com/marketplace/en-US/product/action-rpg)는 `FGameplayEffectContainer`라는 구조체를 구현합니다. 이들은 바닐라 GAS에는 없지만 `GameplayEffects`와 [`TargetData`](#concepts-targeting-data)를 포함하는 데 매우 유용합니다. `GameplayEffects`에서 `GameplayEffectSpecs`를 생성하고 `GameplayEffectContext`에 기본값을 설정하는 등의 일부 노력을 자동화합니다. `GameplayAbility`에서 `GameplayEffectContainer`를 만들고 이를 스폰된 발사체에 전달하는 것은 매우 쉽고 간단합니다. 포함된 샘플 프로젝트에서는 바닐라 GAS에서 어떻게 작업할 것인지 보여주기 위해 `GameplayEffectContainers`를 구현하지 않기로 했지만, 이들을 살펴보고 프로젝트에 추가하는 것을 고려해 보는 것이 좋습니다.

`SetByCallers`를 추가하는 등의 작업을 위해 `GameplayEffectContainers` 내의 `GESpecs`에 접근하려면 `FGameplayEffectContainer`를 분해하고 `GESpecs` 배열의 인덱스로 `GESpec` 참조에 접근하세요. 이는 접근하려는 `GESpec`의 인덱스를 미리 알고 있어야 합니다.

![GameplayEffectContainer와 함께 SetByCaller 사용](https://github.com/tranek/GASDocumentation/raw/master/Images/gecontainersetbycaller.png)

`GameplayEffectContainers`는 또한 [타겟팅](#concepts-targeting-containers)을 위한 선택적이고 효율적인 수단을 포함합니다.

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-ga"></a>
### 4.6 게임플레이 어빌리티

<a name="concepts-ga-definition"></a>
#### 4.6.1 게임플레이 어빌리티 정의
[`GameplayAbilities`](https://docs.unrealengine.com/en-US/API/Plugins/GameplayAbilities/Abilities/UGameplayAbility/index.html) (`GA`)는 게임에서 `Actor`가 수행할 수 있는 모든 행동이나 기술입니다. 예를 들어 달리기와 총 쏘기와 같이 한 번에 둘 이상의 `GameplayAbility`가 활성화될 수 있습니다. 이들은 블루프린트나 C++로 만들 수 있습니다.

`GameplayAbilities`의 예:
* 점프
* 달리기
* 총 쏘기
* X초마다 공격을 수동적으로 막기
* 포션 사용
* 문 열기
* 자원 수집
* 건물 건설

`GameplayAbilities`로 구현하지 않아야 할 것들:
* 기본 이동 입력
* UI와의 일부 상호작용 - 상점에서 아이템을 구매하는 데 `GameplayAbility`를 사용하지 마세요.

이들은 규칙이 아니라 단지 제 추천사항입니다. 여러분의 설계와 구현은 다를 수 있습니다.

`GameplayAbilities`는 어트리뷰트 변경량을 수정하거나 `GameplayAbility`의 기능을 변경하기 위한 레벨을 갖는 기본 기능을 제공합니다.

`GameplayAbilities`는 [`Net Execution Policy`](#concepts-ga-net)에 따라 소유 클라이언트 및/또는 서버에서 실행되지만 시뮬레이션된 프록시에서는 실행되지 않습니다. `Net Execution Policy`는 `GameplayAbility`가 로컬에서 [예측](#concepts-p)될지 여부를 결정합니다. 이들은 [선택적 비용과 쿨다운 `GameplayEffects`](#concepts-ga-commit)에 대한 기본 동작을 포함합니다. `GameplayAbilities`는 이벤트 대기, 어트리뷰트 변경 대기, 플레이어가 타겟을 선택하기를 기다리거나 `Root Motion Source`로 `Character`를 이동시키는 것과 같이 시간이 지남에 따라 발생하는 작업에 [`AbilityTasks`](#concepts-at)를 사용합니다. **시뮬레이션된 클라이언트는 `GameplayAbilities`를 실행하지 않습니다**. 대신, 서버가 능력을 실행할 때 시뮬레이션된 프록시에서 시각적으로 재생되어야 하는 모든 것(애니메이션 몽타주 등)은 `AbilityTasks`나 사운드와 파티클 같은 시각적 요소를 위한 [`GameplayCues`](#concepts-gc)를 통해 복제되거나 RPC될 것입니다.

모든 `GameplayAbilities`는 게임플레이 로직으로 `ActivateAbility()` 함수를 오버라이드할 것입니다. `GameplayAbility`가 완료되거나 취소될 때 실행되는 추가 로직을 `EndAbility()`에 추가할 수 있습니다.

간단한 `GameplayAbility`의 흐름도:
![간단한 GameplayAbility 흐름도](https://github.com/tranek/GASDocumentation/raw/master/Images/abilityflowchartsimple.png)

더 복잡한 `GameplayAbility`의 흐름도:
![복잡한 GameplayAbility 흐름도](https://github.com/tranek/GASDocumentation/raw/master/Images/abilityflowchartcomplex.png)

복잡한 능력은 서로 상호작용하는(활성화, 취소 등) 여러 `GameplayAbilities`를 사용하여 구현할 수 있습니다.

<a name="concepts-ga-definition-reppolicy"></a>
##### 4.6.1.1 복제 정책
이 옵션을 사용하지 마세요. 이름이 오해의 소지가 있고 필요하지 않습니다. [`GameplayAbilitySpecs`](#concepts-ga-spec)는 기본적으로 서버에서 소유 클라이언트로 복제됩니다. 위에서 언급했듯이, **`GameplayAbilities`는 시뮬레이션된 프록시에서 실행되지 않습니다**. 그들은 `AbilityTasks`와 `GameplayCues`를 사용하여 시뮬레이션된 프록시에 시각적 변화를 복제하거나 RPC합니다. Epic의 Dave Ratti는 [미래에 이 옵션을 제거하고 싶다고 말했습니다](https://epicgames.ent.box.com/s/m1egifkxv3he3u3xezb9hzbgroxyhx89).

<a name="concepts-ga-definition-remotecancel"></a>
##### 4.6.1.2 서버가 원격 어빌리티 취소를 존중함
이 옵션은 문제를 더 자주 일으킵니다. 클라이언트의 `GameplayAbility`가 취소되거나 자연스럽게 완료되어 종료되면 서버의 버전이 완료되었는지 여부에 관계없이 강제로 종료된다는 의미입니다. 후자의 문제가 중요한데, 특히 높은 지연 시간을 가진 플레이어가 사용하는 로컬에서 예측된 `GameplayAbilities`의 경우 그렇습니다. 일반적으로 이 옵션을 비활성화하는 것이 좋습니다.

<a name="concepts-ga-definition-repinputdirectly"></a>
##### 4.6.1.3 입력 직접 복제
이 옵션을 설정하면 입력 누름 및 해제 이벤트가 항상 서버로 복제됩니다. Epic은 이를 사용하지 않고 대신 [입력을 `ASC`에 바인딩](#concepts-ga-input)한 경우 기존의 입력 관련 [`AbilityTasks`](#concepts-at)에 내장된 `Generic Replicated Events`를 사용하는 것을 권장합니다.

Epic의 주석:
```c++
/** 직접 입력 상태 복제. 능력에서 bReplicateInputDirectly가 true로 설정된 경우 이들이 호출되며 일반적으로 사용하는 것이 좋지 않습니다. (대신 Generic Replicated Events를 사용하는 것이 좋습니다). */
UAbilitySystemComponent::ServerSetInputPressed()
```

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-ga-input"></a>
#### 4.6.2 ASC에 입력 바인딩
`ASC`는 입력 액션을 직접 바인딩하고 `GameplayAbilities`를 부여할 때 이러한 입력을 할당할 수 있습니다. `GameplayAbilities`에 할당된 입력 액션은 `GameplayTag` 요구사항이 충족되면 누를 때 자동으로 해당 `GameplayAbilities`를 활성화합니다. 할당된 입력 액션은 입력에 응답하는 내장 `AbilityTasks`를 사용하는 데 필요합니다.

`GameplayAbilities`를 활성화하도록 할당된 입력 액션 외에도 `ASC`는 일반 `Confirm` 및 `Cancel` 입력도 받아들입니다. 이러한 특별한 입력은 [`Target Actors`](#concepts-targeting-actors)를 확인하거나 취소하는 등의 작업에 `AbilityTasks`에서 사용됩니다.

`ASC`에 입력을 바인딩하려면 먼저 입력 액션 이름을 바이트로 변환하는 열거형을 만들어야 합니다. 열거형 이름은 프로젝트 설정의 입력 액션 이름과 정확히 일치해야 합니다. `DisplayName`은 중요하지 않습니다.

샘플 프로젝트에서:
```c++
UENUM(BlueprintType)
enum class EGDAbilityInputID : uint8
{
	// 0 None
	None			UMETA(DisplayName = "None"),
	// 1 Confirm
	Confirm			UMETA(DisplayName = "Confirm"),
	// 2 Cancel
	Cancel			UMETA(DisplayName = "Cancel"),
	// 3 LMB
	Ability1		UMETA(DisplayName = "Ability1"),
	// 4 RMB
	Ability2		UMETA(DisplayName = "Ability2"),
	// 5 Q
	Ability3		UMETA(DisplayName = "Ability3"),
	// 6 E
	Ability4		UMETA(DisplayName = "Ability4"),
	// 7 R
	Ability5		UMETA(DisplayName = "Ability5"),
	// 8 Sprint
	Sprint			UMETA(DisplayName = "Sprint"),
	// 9 Jump
	Jump			UMETA(DisplayName = "Jump")
};
```

`ASC`가 `Character`에 있는 경우, `SetupPlayerInputComponent()`에 `ASC`에 바인딩하는 함수를 포함하세요:
```c++
// AbilitySystemComponent에 바인딩
FTopLevelAssetPath AbilityEnumAssetPath = FTopLevelAssetPath(FName("/Script/GASDocumentation"), FName("EGDAbilityInputID"));
AbilitySystemComponent->BindAbilityActivationToInputComponent(PlayerInputComponent, FGameplayAbilityInputBinds(FString("ConfirmTarget"),
	FString("CancelTarget"), AbilityEnumAssetPath, static_cast<int32>(EGDAbilityInputID::Confirm), static_cast<int32>(EGDAbilityInputID::Cancel)));
```

`ASC`가 `PlayerState`에 있는 경우, `SetupPlayerInputComponent()` 내에서 `PlayerState`가 아직 클라이언트에 복제되지 않았을 수 있는 잠재적인 경쟁 조건이 있습니다. 따라서 `SetupPlayerInputComponent()`와 `OnRep_PlayerState()`에서 입력에 바인딩을 시도하는 것을 추천합니다. `OnRep_PlayerState()`만으로는 충분하지 않습니다. `PlayerController`가 클라이언트에게 `ClientRestart()`를 호출하도록 지시하기 전에 `PlayerState`가 복제되어 `InputComponent`를 생성하는 경우가 있을 수 있기 때문입니다. 샘플 프로젝트는 두 위치에서 바인딩을 시도하는 것을 보여주며, 불리언을 사용하여 프로세스를 제어하여 실제로 한 번만 입력을 바인딩합니다.

**참고:** 샘플 프로젝트에서 열거형의 `Confirm`과 `Cancel`은 프로젝트 설정의 입력 액션 이름(`ConfirmTarget`과 `CancelTarget`)과 일치하지 않지만, `BindAbilityActivationToInputComponent()`에서 그들 사이의 매핑을 제공합니다. 이들은 특별한 경우이며 매핑을 제공하므로 일치할 필요가 없지만 일치할 수 있습니다. 열거형의 다른 모든 입력은 프로젝트 설정의 입력 액션 이름과 일치해야 합니다.

하나의 입력으로만 활성화될 `GameplayAbilities`의 경우(MOBA처럼 항상 같은 "슬롯"에 존재할 것), `UGameplayAbility` 서브클래스에 입력을 정의할 수 있는 변수를 추가하는 것을 선호합니다. 그런 다음 능력을 부여할 때 `ClassDefaultObject`에서 이를 읽을 수 있습니다.

<a name="concepts-ga-input-noactivate"></a>
##### 4.6.2.1 어빌리티를 활성화하지 않고 입력에 바인딩하기
입력이 눌렸을 때 `GameplayAbilities`가 자동으로 활성화되지 않게 하면서도 `AbilityTasks`와 함께 사용하기 위해 입력에 바인딩하려면, `UGameplayAbility` 서브클래스에 기본값이 `true`인 새로운 bool 변수 `bActivateOnInput`을 추가하고 `UAbilitySystemComponent::AbilityLocalInputPressed()`를 오버라이드할 수 있습니다.

```c++
void UGSAbilitySystemComponent::AbilityLocalInputPressed(int32 InputID)
{
	// GenericConfirm/Cancel과 중복된 InputID이고 GenericConfim/Cancel 콜백이 바인딩된 경우 입력 소비
	if (IsGenericConfirmInputBound(InputID))
	{
		LocalInputConfirm();
		return;
	}
	if (IsGenericCancelInputBound(InputID))
	{
		LocalInputCancel();
		return;
	}
	// ---------------------------------------------------------
	ABILITYLIST_SCOPE_LOCK();
	for (FGameplayAbilitySpec& Spec : ActivatableAbilities.Items)
	{
		if (Spec.InputID == InputID)
		{
			if (Spec.Ability)
			{
				Spec.InputPressed = true;
				if (Spec.IsActive())
				{
					if (Spec.Ability->bReplicateInputDirectly && IsOwnerActorAuthoritative() == false)
					{
						ServerSetInputPressed(Spec.Handle);
					}
					AbilitySpecInputPressed(Spec);
					// InputPressed 이벤트 호출. 여기서 복제되지 않습니다. 누군가 수신 중이라면 InputPressed 이벤트를 서버로 복제할 수 있습니다.
					InvokeReplicatedEvent(EAbilityGenericReplicatedEvent::InputPressed, Spec.Handle, Spec.ActivationInfo.GetActivationPredictionKey());
				}
				else
				{
					UGSGameplayAbility* GA = Cast<UGSGameplayAbility>(Spec.Ability);
					if (GA && GA->bActivateOnInput)
					{
						// 어빌리티가 활성화되지 않았으므로 활성화 시도
						TryActivateAbility(Spec.Handle);
					}
				}
			}
		}
	}
}
```

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-ga-granting"></a>
#### 4.6.3 어빌리티 부여
`ASC`에 `GameplayAbility`를 부여하면 [`GameplayTag` 요구사항](#concepts-ga-tags)을 충족할 경우 언제든지 `GameplayAbility`를 활성화할 수 있도록 `ASC`의 `ActivatableAbilities` 목록에 추가됩니다.

우리는 서버에서 `GameplayAbilities`를 부여하며, 이는 자동으로 [`GameplayAbilitySpec`](#concepts-ga-spec)을 소유 클라이언트에게 복제합니다. 다른 클라이언트/시뮬레이션된 프록시는 `GameplayAbilitySpec`을 받지 않습니다.

샘플 프로젝트는 `Character` 클래스에 `TArray<TSubclassOf<UGDGameplayAbility>>`를 저장하고 게임 시작 시 이를 읽고 부여합니다:

```c++
void AGDCharacterBase::AddCharacterAbilities()
{
	// 서버에서만 어빌리티 부여
	if (Role != ROLE_Authority || !AbilitySystemComponent.IsValid() || AbilitySystemComponent->bCharacterAbilitiesGiven)
	{
		return;
	}
	for (TSubclassOf<UGDGameplayAbility>& StartupAbility : CharacterAbilities)
	{
		AbilitySystemComponent->GiveAbility(
			FGameplayAbilitySpec(StartupAbility, GetAbilityLevel(StartupAbility.GetDefaultObject()->AbilityID), static_cast<int32>(StartupAbility.GetDefaultObject()->AbilityInputID), this));
	}
	AbilitySystemComponent->bCharacterAbilitiesGiven = true;
}
```

이러한 `GameplayAbilities`를 부여할 때, 우리는 `UGameplayAbility` 클래스, 어빌리티 레벨, 바인딩된 입력, 그리고 이 `GameplayAbility`를 이 `ASC`에 부여한 `SourceObject`로 `GameplayAbilitySpecs`를 생성하고 있습니다.

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-ga-activating"></a>
#### 4.6.4 어빌리티 활성화
`GameplayAbility`에 입력 액션이 할당되면, 입력이 눌리고 `GameplayTag` 요구사항을 충족하면 자동으로 활성화됩니다. 하지만 이것이 항상 `GameplayAbility`를 활성화하는 바람직한 방법은 아닐 수 있습니다. `ASC`는 `GameplayAbilities`를 활성화하는 다른 네 가지 방법을 제공합니다: `GameplayTag`로, `GameplayAbility` 클래스로, `GameplayAbilitySpec` 핸들로, 그리고 이벤트로. 이벤트로 `GameplayAbility`를 활성화하면 [이벤트와 함께 데이터 페이로드를 전달](#concepts-ga-data)할 수 있습니다.

```c++
UFUNCTION(BlueprintCallable, Category = "Abilities")
bool TryActivateAbilitiesByTag(const FGameplayTagContainer& GameplayTagContainer, bool bAllowRemoteActivation = true);

UFUNCTION(BlueprintCallable, Category = "Abilities")
bool TryActivateAbilityByClass(TSubclassOf<UGameplayAbility> InAbilityToActivate, bool bAllowRemoteActivation = true);

bool TryActivateAbility(FGameplayAbilitySpecHandle AbilityToActivate, bool bAllowRemoteActivation = true);

bool TriggerAbilityFromGameplayEvent(FGameplayAbilitySpecHandle AbilityToTrigger, FGameplayAbilityActorInfo* ActorInfo, FGameplayTag Tag, const FGameplayEventData* Payload, UAbilitySystemComponent& Component);

FGameplayAbilitySpecHandle GiveAbilityAndActivateOnce(const FGameplayAbilitySpec& AbilitySpec, const FGameplayEventData* GameplayEventData);
```

이벤트로 `GameplayAbility`를 활성화하려면, `GameplayAbility`의 `Triggers`가 설정되어 있어야 합니다. `GameplayTag`를 할당하고 `GameplayEvent`에 대한 옵션을 선택하세요. 이벤트를 보내려면 `UAbilitySystemBlueprintLibrary::SendGameplayEventToActor(AActor* Actor, FGameplayTag EventTag, FGameplayEventData Payload)` 함수를 사용하세요. 이벤트로 `GameplayAbility`를 활성화하면 데이터와 함께 페이로드를 전달할 수 있습니다.

`GameplayAbility` `Triggers`는 또한 `GameplayTag`가 추가되거나 제거될 때 `GameplayAbility`를 활성화할 수 있게 합니다.

**참고:** 블루프린트에서 이벤트로 `GameplayAbility`를 활성화할 때는 반드시 `ActivateAbilityFromEvent` 노드를 사용해야 합니다.

**참고:** 패시브 어빌리티처럼 항상 실행되는 `GameplayAbility`가 아니라면 `GameplayAbility`가 종료되어야 할 때 `EndAbility()`를 호출하는 것을 잊지 마세요.

**로컬에서 예측된** `GameplayAbilities`의 활성화 순서:
1. **소유 클라이언트**가 `TryActivateAbility()` 호출
2. `InternalTryActivateAbility()` 호출
3. `CanActivateAbility()` 호출 및 `GameplayTag` 요구사항 충족 여부, `ASC`가 비용을 감당할 수 있는지, `GameplayAbility`가 쿨다운 중이 아닌지, 다른 인스턴스가 현재 활성화되어 있지 않은지 반환
4. `CallServerTryActivateAbility()` 호출 및 생성한 `Prediction Key` 전달
5. `CallActivateAbility()` 호출
6. `PreActivate()` 호출 (Epic은 이를 "상용구 초기화 작업"이라고 부름)
7. `ActivateAbility()` 호출, 마침내 어빌리티 활성화

**서버**가 `CallServerTryActivateAbility()` 수신
1. `ServerTryActivateAbility()` 호출
2. `InternalServerTryActivateAbility()` 호출 
3. `InternalTryActivateAbility()` 호출
4. `CanActivateAbility()` 호출 및 `GameplayTag` 요구사항 충족 여부, `ASC`가 비용을 감당할 수 있는지, `GameplayAbility`가 쿨다운 중이 아닌지, 다른 인스턴스가 현재 활성화되어 있지 않은지 반환
5. 성공 시 `ClientActivateAbilitySucceed()` 호출, 활성화가 서버에 의해 확인되었음을 알리는 `ActivationInfo` 업데이트 및 `OnConfirmDelegate` 델리게이트 브로드캐스트. 이는 입력 확인과는 다릅니다.
6. `CallActivateAbility()` 호출
7. `PreActivate()` 호출 (Epic은 이를 "상용구 초기화 작업"이라고 부름)
8. `ActivateAbility()` 호출, 마침내 어빌리티 활성화

서버가 활성화에 실패하면 언제든지 `ClientActivateAbilityFailed()`를 호출하여 클라이언트의 `GameplayAbility`를 즉시 종료하고 예측된 변경사항을 되돌립니다.

<a name="concepts-ga-activating-passive"></a>
##### 4.6.4.1 패시브 어빌리티
자동으로 활성화되고 지속적으로 실행되는 패시브 `GameplayAbilities`를 구현하려면, `UGameplayAbility::OnAvatarSet()`을 오버라이드하세요. 이 함수는 `GameplayAbility`가 부여되고 `AvatarActor`가 설정될 때 자동으로 호출됩니다. 그리고 `TryActivateAbility()`를 호출하세요.

사용자 정의 `UGameplayAbility` 클래스에 `GameplayAbility`가 부여될 때 활성화되어야 하는지 지정하는 `bool`을 추가하는 것을 권장합니다. 샘플 프로젝트는 패시브 방어구 스택 능력에 대해 이를 수행합니다.

패시브 `GameplayAbilities`는 일반적으로 [`Net Execution Policy`](#concepts-ga-net)가 `Server Only`입니다.

```c++
void UGDGameplayAbility::OnAvatarSet(const FGameplayAbilityActorInfo * ActorInfo, const FGameplayAbilitySpec & Spec)
{
	Super::OnAvatarSet(ActorInfo, Spec);

	if (bActivateAbilityOnGranted)
	{
		ActorInfo->AbilitySystemComponent->TryActivateAbility(Spec.Handle, false);
	}
}
```

Epic은 이 함수를 패시브 능력을 시작하고 `BeginPlay` 유형의 작업을 수행하는 올바른 장소로 설명합니다.

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-ga-activating-failedtags"></a>
##### 4.6.4.2 활성화 실패 태그

어빌리티에는 어빌리티 활성화가 실패한 이유를 알려주는 기본 로직이 있습니다. 이를 활성화하려면 기본 실패 사례에 해당하는 GameplayTags를 설정해야 합니다.

프로젝트에 이러한 태그(또는 자체 명명 규칙)를 추가하세요:
```
+GameplayTagList=(Tag="Activation.Fail.BlockedByTags",DevComment="")
+GameplayTagList=(Tag="Activation.Fail.CantAffordCost",DevComment="")
+GameplayTagList=(Tag="Activation.Fail.IsDead",DevComment="")
+GameplayTagList=(Tag="Activation.Fail.MissingTags",DevComment="")
+GameplayTagList=(Tag="Activation.Fail.Networking",DevComment="")
+GameplayTagList=(Tag="Activation.Fail.OnCooldown",DevComment="")
```

그런 다음 [`GASDocumentation\Config\DefaultGame.ini`](https://github.com/tranek/GASDocumentation/blob/master/Config/DefaultGame.ini#L8-L13)에 추가하세요:
```
[/Script/GameplayAbilities.AbilitySystemGlobals]
ActivateFailIsDeadName=Activation.Fail.IsDead
ActivateFailCooldownName=Activation.Fail.OnCooldown
ActivateFailCostName=Activation.Fail.CantAffordCost
ActivateFailTagsBlockedName=Activation.Fail.BlockedByTags
ActivateFailTagsMissingName=Activation.Fail.MissingTags
ActivateFailNetworkingName=Activation.Fail.Networking
```

이제 어빌리티 활성화가 실패할 때마다 해당 GameplayTag가 출력 로그 메시지에 포함되거나 `showdebug AbilitySystem` HUD에 표시됩니다.
```
LogAbilitySystem: Display: InternalServerTryActivateAbility. Rejecting ClientActivation of Default__GA_FireGun_C. InternalTryActivateAbility failed: Activation.Fail.BlockedByTags
LogAbilitySystem: Display: ClientActivateAbilityFailed_Implementation. PredictionKey :109 Ability: Default__GA_FireGun_C
```

![showdebug AbilitySystem에 표시된 활성화 실패 태그](https://github.com/tranek/GASDocumentation/raw/master/Images/activationfailedtags.png)

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-ga-cancelabilities"></a>
#### 4.6.5 어빌리티 취소하기
내부에서 `GameplayAbility`를 취소하려면 `CancelAbility()`를 호출합니다. 이는 `EndAbility()`를 호출하고 그 `WasCancelled` 매개변수를 true로 설정합니다.

외부에서 `GameplayAbility`를 취소하기 위해 `ASC`는 몇 가지 함수를 제공합니다:

```c++
/** 지정된 어빌리티 CDO를 취소합니다. */
void CancelAbility(UGameplayAbility* Ability);	

/** 전달된 스펙 핸들로 표시된 어빌리티를 취소합니다. 핸들이 재활성화된 어빌리티 중에 없으면 아무 일도 일어나지 않습니다. */
void CancelAbilityHandle(const FGameplayAbilitySpecHandle& AbilityHandle);

/** 지정된 태그를 가진 모든 어빌리티를 취소합니다. Ignore 인스턴스는 취소하지 않습니다 */
void CancelAbilities(const FGameplayTagContainer* WithTags=nullptr, const FGameplayTagContainer* WithoutTags=nullptr, UGameplayAbility* Ignore=nullptr);

/** 태그에 관계없이 모든 어빌리티를 취소합니다. ignore 인스턴스는 취소하지 않습니다 */
void CancelAllAbilities(UGameplayAbility* Ignore=nullptr);

/** 모든 어빌리티를 취소하고 남아있는 인스턴스화된 어빌리티를 제거합니다 */
virtual void DestroyActiveState();
```

**참고:** `Non-Instanced` `GameplayAbilities`가 있는 경우 `CancelAllAbilities`가 제대로 작동하지 않는 것 같습니다. `Non-Instanced` `GameplayAbility`를 만나면 포기하는 것 같습니다. `CancelAbilities`는 `Non-Instanced` `GameplayAbilities`를 더 잘 처리할 수 있으며 샘플 프로젝트에서 이를 사용합니다(Jump는 non-instanced `GameplayAbility`입니다). 여러분의 경험은 다를 수 있습니다.

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-ga-definition-activeability"></a>
#### 4.6.6 활성 어빌리티 가져오기
초보자들은 종종 "활성 어빌리티를 어떻게 가져올 수 있나요?"라고 묻습니다. 아마도 변수를 설정하거나 취소하기 위해서일 것입니다. 한 번에 둘 이상의 `GameplayAbility`가 활성화될 수 있으므로 하나의 "활성 어빌리티"는 없습니다. 대신, `ASC`의 `ActivatableAbilities` 목록(ASC가 소유한 부여된 `GameplayAbilities`)을 검색하여 찾고 있는 [`Asset` 또는 `Granted` `GameplayTag`](#concepts-ga-tags)와 일치하는 것을 찾아야 합니다.

`UAbilitySystemComponent::GetActivatableAbilities()`는 반복할 수 있는 `TArray<FGameplayAbilitySpec>`을 반환합니다.

`ASC`는 또한 수동으로 `GameplayAbilitySpecs` 목록을 반복하는 대신 매개변수로 `GameplayTagContainer`를 받아 검색을 돕는 다른 헬퍼 함수를 가지고 있습니다. `bOnlyAbilitiesThatSatisfyTagRequirements` 매개변수는 `GameplayTag` 요구사항을 만족하고 지금 바로 활성화될 수 있는 `GameplayAbilitySpecs`만 반환합니다. 예를 들어, 무기를 들고 있을 때와 맨손일 때 각각 다른 기본 공격 `GameplayAbilities`를 가질 수 있고, 무기 장착 여부에 따라 `GameplayTag` 요구사항을 설정하여 올바른 것이 활성화됩니다. 자세한 정보는 함수에 대한 Epic의 주석을 참조하세요.
```c++
UAbilitySystemComponent::GetActivatableGameplayAbilitySpecsByAllMatchingTags(const FGameplayTagContainer& GameplayTagContainer, TArray < struct FGameplayAbilitySpec* >& MatchingGameplayAbilities, bool bOnlyAbilitiesThatSatisfyTagRequirements = true)
```

찾고 있는 `FGameplayAbilitySpec`을 얻으면 그것에 대해 `IsActive()`를 호출할 수 있습니다.

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-ga-instancing"></a>
#### 4.6.7 인스턴싱 정책
`GameplayAbility`의 `Instancing Policy`는 활성화될 때 `GameplayAbility`가 인스턴스화되는지와 어떻게 인스턴스화되는지를 결정합니다.

| `Instancing Policy`     | 설명                                                                                 | 사용 예시                                                                                                                                                                                                                                                                                                        |
| ----------------------- | ------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Instanced Per Actor     | 각 `ASC`는 활성화 사이에 재사용되는 `GameplayAbility`의 인스턴스를 하나만 가집니다.  | 이는 가장 많이 사용하게 될 `Instancing Policy`일 것입니다. 어떤 능력에도 사용할 수 있으며 활성화 사이에 지속성을 제공합니다. 디자이너는 필요한 경우 활성화 사이에 변수를 수동으로 재설정해야 합니다.                                                                                                           |
| Instanced Per Execution | `GameplayAbility`가 활성화될 때마다 새로운 `GameplayAbility` 인스턴스가 생성됩니다.  | 이러한 `GameplayAbilities`의 이점은 활성화할 때마다 변수가 재설정된다는 것입니다. 활성화할 때마다 새로운 `GameplayAbilities`를 생성하므로 `Instanced Per Actor`보다 성능이 떨어집니다. 샘플 프로젝트는 이들 중 어느 것도 사용하지 않습니다.                                                                     |
| Non-Instanced           | `GameplayAbility`는 `ClassDefaultObject`에서 작동합니다. 인스턴스가 생성되지 않습니다. | 이는 세 가지 중 가장 좋은 성능을 가지지만 수행할 수 있는 작업에 가장 제한적입니다. `Non-Instanced` `GameplayAbilities`는 상태를 저장할 수 없습니다. 즉, 동적 변수와 `AbilityTask` 델리게이트에 바인딩할 수 없습니다. MOBA나 RTS의 미니언 기본 공격과 같은 자주 사용되는 간단한 능력에 가장 적합합니다. 샘플 프로젝트의 Jump `GameplayAbility`는 `Non-Instanced`입니다. |

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-ga-net"></a>
#### 4.6.8 넷 실행 정책
`GameplayAbility`의 `Net Execution Policy`는 누가 `GameplayAbility`를 실행하고 어떤 순서로 실행하는지 결정합니다.

| `Net Execution Policy` | 설명                                                                                                                                                                             |
| ---------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Local Only`           | `GameplayAbility`는 소유 클라이언트에서만 실행됩니다. 이는 로컬 시각적 변경만 하는 능력에 유용할 수 있습니다. 싱글 플레이어 게임은 `Server Only`를 사용해야 합니다.              |
| `Local Predicted`      | `Local Predicted` `GameplayAbilities`는 먼저 소유 클라이언트에서 실행된 다음 서버에서 실행됩니다. 서버 버전은 클라이언트가 잘못 예측한 것을 수정합니다. [예측](#concepts-p) 참조. |
| `Server Only`          | `GameplayAbility`는 서버에서만 실행됩니다. 패시브 `GameplayAbilities`는 일반적으로 `Server Only`가 될 것입니다. 싱글 플레이어 게임은 이것을 사용해야 합니다.                     |
| `Server Initiated`     | `Server Initiated` `GameplayAbilities`는 먼저 서버에서 실행된 다음 소유 클라이언트에서 실행됩니다. 개인적으로 이것을 많이 사용하지 않았습니다.                                   | 

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-ga-tags"></a>
#### 4.6.9 어빌리티 태그
`GameplayAbilities`는 내장된 로직을 가진 `GameplayTagContainers`와 함께 제공됩니다. 이 `GameplayTags` 중 어느 것도 복제되지 않습니다.

| `GameplayTag Container`     | 설명                                                                                                                                                                   |
| --------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Ability Tags`              | `GameplayAbility`가 소유한 `GameplayTags`. 이들은 단순히 `GameplayAbility`를 설명하는 `GameplayTags`입니다.                                                             |
| `Cancel Abilities with Tag` | 이 `GameplayAbility`가 활성화될 때 `Ability Tags`에 이 `GameplayTags`를 가진 다른 `GameplayAbilities`가 취소됩니다.                                                     |
| `Block Abilities with Tag`  | 이 `GameplayAbility`가 활성 상태인 동안 `Ability Tags`에 이 `GameplayTags`를 가진 다른 `GameplayAbilities`의 활성화가 차단됩니다.                                       |
| `Activation Owned Tags`     | 이 `GameplayAbility`가 활성 상태인 동안 `GameplayAbility`의 소유자에게 이 `GameplayTags`가 주어집니다. 이들은 복제되지 않는다는 점을 기억하세요.                        |
| `Activation Required Tags`  | 이 `GameplayAbility`는 소유자가 이 `GameplayTags`를 **모두** 가지고 있을 때만 활성화될 수 있습니다.                                                                     |
| `Activation Blocked Tags`   | 이 `GameplayAbility`는 소유자가 이 `GameplayTags` 중 **하나라도** 가지고 있으면 활성화될 수 없습니다.                                                                   |
| `Source Required Tags`      | 이 `GameplayAbility`는 `Source`가 이 `GameplayTags`를 **모두** 가지고 있을 때만 활성화될 수 있습니다. `Source` `GameplayTags`는 `GameplayAbility`가 이벤트에 의해 트리거된 경우에만 설정됩니다. |
| `Source Blocked Tags`       | 이 `GameplayAbility`는 `Source`가 이 `GameplayTags` 중 **하나라도** 가지고 있으면 활성화될 수 없습니다. `Source` `GameplayTags`는 `GameplayAbility`가 이벤트에 의해 트리거된 경우에만 설정됩니다. |
| `Target Required Tags`      | 이 `GameplayAbility`는 `Target`이 이 `GameplayTags`를 **모두** 가지고 있을 때만 활성화될 수 있습니다. `Target` `GameplayTags`는 `GameplayAbility`가 이벤트에 의해 트리거된 경우에만 설정됩니다. |
| `Target Blocked Tags`       | 이 `GameplayAbility`는 `Target`이 이 `GameplayTags` 중 **하나라도** 가지고 있으면 활성화될 수 없습니다. `Target` `GameplayTags`는 `GameplayAbility`가 이벤트에 의해 트리거된 경우에만 설정됩니다. |

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-ga-spec"></a>
#### 4.6.10 게임플레이 어빌리티 스펙
`GameplayAbilitySpec`은 `GameplayAbility`가 부여된 후 `ASC`에 존재하며 활성화 가능한 `GameplayAbility`를 정의합니다 - `GameplayAbility` 클래스, 레벨, 입력 바인딩, 그리고 `GameplayAbility` 클래스와 별도로 유지되어야 하는 런타임 상태를 포함합니다.

`GameplayAbility`가 서버에서 부여되면, 서버는 `GameplayAbilitySpec`을 소유 클라이언트에게 복제하여 클라이언트가 이를 활성화할 수 있도록 합니다.

`GameplayAbilitySpec`을 활성화하면 `Instancing Policy`에 따라 `GameplayAbility`의 인스턴스가 생성됩니다(`Non-Instanced` `GameplayAbilities`의 경우 생성되지 않음).

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-ga-data"></a>
#### 4.6.11 어빌리티에 데이터 전달하기
`GameplayAbilities`의 일반적인 패러다임은 `활성화->데이터 생성->적용->종료`입니다. 때로는 기존 데이터에 대해 작업해야 할 수 있습니다. GAS는 외부 데이터를 `GameplayAbilities`에 가져오기 위한 몇 가지 옵션을 제공합니다:

| 방법                                           | 설명                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| ---------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 이벤트로 `GameplayAbility` 활성화              | 데이터 페이로드를 포함한 이벤트로 `GameplayAbility`를 활성화합니다. 이벤트의 페이로드는 로컬 예측 `GameplayAbilities`를 위해 클라이언트에서 서버로 복제됩니다. 기존 변수에 맞지 않는 임의의 데이터에 대해 두 개의 `Optional Object` 또는 [`TargetData`](#concepts-targeting-data) 변수를 사용하세요. 이 방법의 단점은 입력 바인딩으로 어빌리티를 활성화할 수 없다는 것입니다. 이벤트로 `GameplayAbility`를 활성화하려면 `GameplayAbility`의 `Triggers`가 설정되어 있어야 합니다. `GameplayTag`를 할당하고 `GameplayEvent`에 대한 옵션을 선택하세요. 이벤트를 보내려면 `UAbilitySystemBlueprintLibrary::SendGameplayEventToActor(AActor* Actor, FGameplayTag EventTag, FGameplayEventData Payload)` 함수를 사용하세요. |
| `WaitGameplayEvent` `AbilityTask` 사용         | `WaitGameplayEvent` `AbilityTask`를 사용하여 `GameplayAbility`에게 활성화 후 페이로드 데이터가 있는 이벤트를 수신하도록 지시합니다. 이벤트 페이로드와 전송 프로세스는 이벤트로 `GameplayAbilities`를 활성화하는 것과 동일합니다. 이 방법의 단점은 이벤트가 `AbilityTask`에 의해 복제되지 않으므로 `Local Only`와 `Server Only` `GameplayAbilities`에만 사용해야 한다는 것입니다. 잠재적으로 이벤트 페이로드를 복제하는 자체 `AbilityTask`를 작성할 수 있습니다.                                                                                                                                                                                                                               |
| `TargetData` 사용                              | 사용자 정의 `TargetData` 구조체는 클라이언트와 서버 간에 임의의 데이터를 전달하는 좋은 방법입니다.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| `OwnerActor` 또는 `AvatarActor`에 데이터 저장  | `OwnerActor`, `AvatarActor` 또는 참조할 수 있는 다른 객체에 저장된 복제된 변수를 사용합니다. 이 방법은 가장 유연하며 입력 바인딩으로 활성화된 `GameplayAbilities`에서도 작동합니다. 그러나 사용 시점에 복제로 인한 데이터 동기화를 보장하지 않습니다. 이를 미리 보장해야 합니다 - 즉, 복제된 변수를 설정한 다음 즉시 `GameplayAbility`를 활성화하면 잠재적인 패킷 손실로 인해 수신자 측에서 발생하는 순서가 보장되지 않습니다.                                                                                                                                                                                                                                                                        |

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-ga-commit"></a>
#### 4.6.12 어빌리티 비용과 쿨다운
`GameplayAbilities`는 선택적 비용과 쿨다운 기능을 제공합니다. 비용은 `GameplayAbility`를 활성화하기 위해 `ASC`가 가져야 하는 미리 정의된 `Attributes` 양으로, `Instant` `GameplayEffect`([`Cost GE`](#concepts-ge-cost))로 구현됩니다. 쿨다운은 만료될 때까지 `GameplayAbility`의 재활성화를 방지하는 타이머로, `Duration` `GameplayEffect`([`Cooldown GE`](#concepts-ge-cooldown))로 구현됩니다.

`GameplayAbility`가 `UGameplayAbility::Activate()`를 호출하기 전에 `UGameplayAbility::CanActivateAbility()`를 호출합니다. 이 함수는 소유 `ASC`가 비용을 감당할 수 있는지 확인하고(`UGameplayAbility::CheckCost()`) `GameplayAbility`가 쿨다운 중이 아닌지 확인합니다(`UGameplayAbility::CheckCooldown()`).

`GameplayAbility`가 `Activate()`를 호출한 후, `UGameplayAbility::CommitAbility()`를 사용하여 언제든지 선택적으로 비용과 쿨다운을 커밋할 수 있습니다. 이는 `UGameplayAbility::CommitCost()`와 `UGameplayAbility::CommitCooldown()`을 호출합니다. 디자이너는 동시에 커밋되어서는 안 되는 경우 `CommitCost()` 또는 `CommitCooldown()`을 별도로 호출할 수 있습니다. 비용과 쿨다운을 커밋하면 `CheckCost()`와 `CheckCooldown()`을 한 번 더 호출하며, 이는 `GameplayAbility`가 관련하여 실패할 수 있는 마지막 기회입니다. 소유 `ASC`의 `Attributes`는 `GameplayAbility`가 활성화된 후 잠재적으로 변경될 수 있어 커밋 시점에 비용을 충족하지 못할 수 있습니다. 비용과 쿨다운의 커밋은 커밋 시점에 [예측 키](#concepts-p-key)가 유효한 경우 [로컬에서 예측](#concepts-p)될 수 있습니다.

구현 세부 사항은 [`CostGE`](#concepts-ge-cost)와 [`CooldownGE`](#concepts-ge-cooldown)를 참조하세요.

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-ga-leveling"></a>
#### 4.6.13 어빌리티 레벨 업
어빌리티의 레벨을 올리는 두 가지 일반적인 방법이 있습니다:

| 레벨 업 방법                            | 설명                                                                                                                                                                          |
| --------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 취소 후 새 레벨로 재부여                | 서버에서 `ASC`에서 `GameplayAbility`를 취소(제거)하고 다음 레벨로 다시 부여합니다. 이는 당시 활성 상태였던 `GameplayAbility`를 종료합니다.                                    |
| `GameplayAbilitySpec`의 레벨 증가       | 서버에서 `GameplayAbilitySpec`을 찾아 레벨을 증가시키고 더티로 표시하여 소유 클라이언트에 복제되도록 합니다. 이 방법은 당시 활성 상태였던 `GameplayAbility`를 종료하지 않습니다. |

두 방법의 주요 차이점은 레벨 업 시 활성 `GameplayAbilities`를 취소할지 여부입니다. `GameplayAbilities`에 따라 두 방법을 모두 사용하게 될 것입니다. `UGameplayAbility` 서브클래스에 어떤 방법을 사용할지 지정하는 `bool`을 추가하는 것을 추천합니다.

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-ga-sets"></a>
#### 4.6.14 어빌리티 세트
`GameplayAbilitySets`는 캐릭터의 입력 바인딩과 시작 `GameplayAbilities` 목록을 보유하고 `GameplayAbilities`를 부여하는 로직을 포함하는 편리한 `UDataAsset` 클래스입니다. 서브클래스는 추가 로직이나 속성을 포함할 수도 있습니다. Paragon은 각 영웅마다 주어진 모든 `GameplayAbilities`를 포함하는 `GameplayAbilitySet`를 가지고 있었습니다.

지금까지 본 바로는 이 클래스가 불필요하다고 생각합니다. 샘플 프로젝트는 `GameplayAbilitySets`의 모든 기능을 `GDCharacterBase`와 그 서브클래스 내부에서 처리합니다.

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-ga-batching"></a>
#### 4.6.15 어빌리티 배칭
전통적인 `Gameplay Ability` 라이프사이클은 클라이언트에서 서버로 최소 두 개 또는 세 개의 RPC를 포함합니다.

1. `CallServerTryActivateAbility()`
2. `ServerSetReplicatedTargetData()` (선택사항)
3. `ServerEndAbility()`

`GameplayAbility`가 한 프레임에서 이 모든 작업을 하나의 원자적 그룹으로 수행하는 경우, 이 워크플로우를 최적화하여 두 개 또는 세 개의 RPC를 하나의 RPC로 배치(결합)할 수 있습니다. `GAS`는 이 RPC 최적화를 `Ability Batching`이라고 부릅니다. `Ability Batching`을 사용하는 일반적인 예는 히트스캔 총입니다. 히트스캔 총은 한 프레임에서 하나의 원자적 그룹으로 활성화, 선 추적 수행, [`TargetData`](#concepts-targeting-data)를 서버로 전송, 어빌리티 종료를 수행합니다. [GASShooter](https://github.com/tranek/GASShooter) 샘플 프로젝트는 히트스캔 총에 대해 이 기술을 보여줍니다.

반자동 총은 가장 좋은 시나리오이며 `CallServerTryActivateAbility()`, `ServerSetReplicatedTargetData()` (총알 명중 결과), `ServerEndAbility()`를 세 개의 RPC 대신 하나의 RPC로 배치합니다.

완전 자동/연발 총은 첫 번째 총알에 대해 `CallServerTryActivateAbility()`와 `ServerSetReplicatedTargetData()`를 두 개의 RPC 대신 하나의 RPC로 배치합니다. 각 후속 총알은 자체 `ServerSetReplicatedTargetData()` RPC입니다. 마지막으로, 총 발사가 멈추면 `ServerEndAbility()`가 별도의 RPC로 전송됩니다. 이는 첫 번째 총알에 대해 두 개 대신 하나의 RPC만 절약하는 최악의 시나리오입니다. 이 시나리오는 [`Gameplay Event`](#concepts-ga-data)를 통해 어빌리티를 활성화하여 구현할 수도 있었습니다. 이 경우 총알의 `TargetData`를 클라이언트에서 서버로 `EventPayload`와 함께 전송합니다. 후자 접근 방식의 단점은 `TargetData`를 어빌리티 외부에서 생성해야 하는 반면, 배칭 접근 방식은 어빌리티 내부에서 `TargetData`를 생성한다는 것입니다.

`Ability Batching`은 기본적으로 [`ASC`](#concepts-asc)에서 비활성화되어 있습니다. `Ability Batching`을 활성화하려면 `ShouldDoServerAbilityRPCBatch()`를 오버라이드하여 true를 반환하도록 합니다:

```c++
virtual bool ShouldDoServerAbilityRPCBatch() const override { return true; }
```

이제 `Ability Batching`이 활성화되었으므로, 배치하려는 어빌리티를 활성화하기 전에 `FScopedServerAbilityRPCBatcher` 구조체를 미리 생성해야 합니다. 이 특별한 구조체는 그 범위 내에서 이어지는 모든 어빌리티를 배치하려고 시도합니다. `FScopedServerAbilityRPCBatcher`가 범위를 벗어나면, 활성화된 어빌리티는 배치를 시도하지 않습니다. `FScopedServerAbilityRPCBatcher`는 배치될 수 있는 각 함수에 특별한 코드를 가지고 있어 RPC 전송을 가로채고 대신 메시지를 배치 구조체에 패킹합니다. `FScopedServerAbilityRPCBatcher`가 범위를 벗어나면, `UAbilitySystemComponent::EndServerAbilityRPCBatch()`에서 이 배치 구조체를 서버로 자동으로 RPC합니다. 서버는 `UAbilitySystemComponent::ServerAbilityRPCBatch_Internal(FServerAbilityRPCBatch& BatchInfo)`에서 배치 RPC를 받습니다. `BatchInfo` 매개변수는 어빌리티가 종료되어야 하는지, 활성화 시 입력이 눌렸는지에 대한 플래그와 포함된 경우 `TargetData`를 포함합니다. 배칭이 제대로 작동하는지 확인하려면 이 함수에 중단점을 설정하는 것이 좋습니다. 또는 `AbilitySystem.ServerRPCBatching.Log 1` cvar를 사용하여 특별한 어빌리티 배칭 로깅을 활성화할 수 있습니다.

이 메커니즘은 C++에서만 수행할 수 있으며 `FGameplayAbilitySpecHandle`로만 어빌리티를 활성화할 수 있습니다.

```c++
bool UGSAbilitySystemComponent::BatchRPCTryActivateAbility(FGameplayAbilitySpecHandle InAbilityHandle, bool EndAbilityImmediately)
{
	bool AbilityActivated = false;
	if (InAbilityHandle.IsValid())
	{
		FScopedServerAbilityRPCBatcher GSAbilityRPCBatcher(this, InAbilityHandle);
		AbilityActivated = TryActivateAbility(InAbilityHandle, true);

		if (EndAbilityImmediately)
		{
			FGameplayAbilitySpec* AbilitySpec = FindAbilitySpecFromHandle(InAbilityHandle);
			if (AbilitySpec)
			{
				UGSGameplayAbility* GSAbility = Cast<UGSGameplayAbility>(AbilitySpec->GetPrimaryInstance());
				GSAbility->ExternalEndAbility();
			}
		}

		return AbilityActivated;
	}

	return AbilityActivated;
}
```

GASShooter는 반자동 및 완전 자동 총에 대해 동일한 배치된 `GameplayAbility`를 재사용합니다. 이 어빌리티는 `EndAbility()`를 직접 호출하지 않습니다(현재 발사 모드에 따라 배치된 어빌리티에 대한 호출과 플레이어 입력을 관리하는 로컬 전용 어빌리티에 의해 어빌리티 외부에서 처리됩니다). 모든 RPC는 `FScopedServerAbilityRPCBatcher`의 범위 내에서 발생해야 하므로, 제어/관리하는 로컬 전용 어빌리티가 이 어빌리티가 `EndAbility()` 호출을 배치해야 하는지(반자동) 또는 배치하지 않아야 하는지(완전 자동)를 지정할 수 있도록 `EndAbilityImmediately` 매개변수를 제공합니다. 후자의 경우 `EndAbility()` 호출은 나중에 자체 RPC로 발생합니다.

GASShooter는 앞서 언급한 로컬 전용 어빌리티가 배치된 어빌리티를 트리거하는 데 사용하는 블루프린트 노드를 노출합니다.

![배치된 어빌리티 활성화](https://github.com/tranek/GASDocumentation/raw/master/Images/batchabilityactivate.png)

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-ga-netsecuritypolicy"></a>
#### 4.6.16 네트워크 보안 정책
`GameplayAbility`의 `NetSecurityPolicy`는 네트워크에서 어빌리티가 어디서 실행되어야 하는지 결정합니다. 이는 클라이언트가 제한된 어빌리티를 실행하려는 시도로부터 보호합니다.

| `NetSecurityPolicy`     | 설명                                                                                                                                 |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| `ClientOrServer`        | 보안 요구사항 없음. 클라이언트나 서버가 이 어빌리티의 실행과 종료를 자유롭게 트리거할 수 있습니다.                                   |
| `ServerOnlyExecution`   | 이 어빌리티의 실행을 요청하는 클라이언트는 서버에 의해 무시됩니다. 클라이언트는 여전히 서버에 이 어빌리티의 취소나 종료를 요청할 수 있습니다. |
| `ServerOnlyTermination` | 이 어빌리티의 취소나 종료를 요청하는 클라이언트는 서버에 의해 무시됩니다. 클라이언트는 여전히 어빌리티의 실행을 요청할 수 있습니다.      |
| `ServerOnly`            | 서버가 이 어빌리티의 실행과 종료를 모두 제어합니다. 클라이언트의 모든 요청은 무시됩니다.                                              |

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-at"></a>
### 4.7 어빌리티 태스크

<a name="concepts-at-definition"></a>
### 4.7.1 어빌리티 태스크 정의
`GameplayAbilities`는 한 프레임에서만 실행됩니다. 이것만으로는 많은 유연성을 제공하지 않습니다. 시간이 지남에 따라 발생하거나 나중에 특정 시점에 발생하는 델리게이트에 응답해야 하는 작업을 수행하기 위해 `AbilityTasks`라는 지연 작업을 사용합니다.

GAS는 기본적으로 많은 `AbilityTasks`를 제공합니다:
* `RootMotionSource`로 캐릭터를 움직이는 태스크
* 애니메이션 몽타주를 재생하는 태스크
* `Attribute` 변경에 응답하는 태스크
* `GameplayEffect` 변경에 응답하는 태스크
* 플레이어 입력에 응답하는 태스크
* 그 외 다수

`UAbilityTask` 생성자는 동시에 실행되는 `AbilityTasks`의 게임 전체 최대 수를 1000개로 제한합니다. RTS 게임과 같이 동시에 수백 개의 캐릭터가 월드에 존재할 수 있는 게임의 `GameplayAbilities`를 설계할 때 이 점을 유의해야 합니다.

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-at-definition"></a>
### 4.7.2 사용자 정의 어빌리티 태스크
종종 자신만의 사용자 정의 `AbilityTasks`를 만들게 될 것입니다(C++에서). 샘플 프로젝트는 두 개의 사용자 정의 `AbilityTasks`를 포함하고 있습니다:
1. `PlayMontageAndWaitForEvent`는 기본 `PlayMontageAndWait`와 `WaitGameplayEvent` `AbilityTasks`의 조합입니다. 이를 통해 애니메이션 몽타주가 `AnimNotifies`에서 게임플레이 이벤트를 시작한 `GameplayAbility`로 다시 보낼 수 있습니다. 이를 사용하여 애니메이션 몽타주 중 특정 시간에 동작을 트리거합니다.
2. `WaitReceiveDamage`는 `OwnerActor`가 데미지를 받는 것을 수신합니다. 패시브 방어구 스택 `GameplayAbility`는 영웅이 데미지 인스턴스를 받을 때 방어구 스택을 하나 제거합니다.

`AbilityTasks`는 다음으로 구성됩니다:
* `AbilityTask`의 새 인스턴스를 생성하는 정적 함수
* `AbilityTask`가 목적을 달성했을 때 브로드캐스트되는 델리게이트
* 주요 작업을 시작하고 외부 델리게이트에 바인딩하는 등의 `Activate()` 함수
* 바인딩한 외부 델리게이트를 포함한 정리를 위한 `OnDestroy()` 함수
* 바인딩한 모든 외부 델리게이트에 대한 콜백 함수
* 멤버 변수 및 내부 헬퍼 함수

**참고:** `AbilityTasks`는 한 가지 유형의 출력 델리게이트만 선언할 수 있습니다. 매개변수를 사용하는지 여부와 관계없이 모든 출력 델리게이트는 이 유형이어야 합니다. 사용하지 않는 델리게이트 매개변수에 대해서는 기본값을 전달하세요.

`AbilityTasks`는 소유 `GameplayAbility`를 실행하는 클라이언트나 서버에서만 실행됩니다. 그러나 `AbilityTask` 생성자에서 `bSimulatedTask = true;`를 설정하고, `virtual void InitSimulatedTask(UGameplayTasksComponent& InGameplayTasksComponent);`를 오버라이드하고, 모든 멤버 변수를 복제되도록 설정하여 시뮬레이션된 클라이언트에서 실행되도록 설정할 수 있습니다. 이는 모든 움직임 변경을 복제하지 않고 전체 움직임 `AbilityTask`를 시뮬레이션하려는 움직임 `AbilityTasks`와 같은 드문 상황에서만 유용합니다. 모든 `RootMotionSource` `AbilityTasks`가 이를 수행합니다. 예시로 `AbilityTask_MoveToLocation.h/.cpp`를 참조하세요.

`AbilityTask` 생성자에서 `bTickingTask = true;`를 설정하고 `virtual void TickTask(float DeltaTime);`를 오버라이드하면 `AbilityTasks`가 `Tick`할 수 있습니다. 이는 프레임 간에 값을 부드럽게 보간해야 할 때 유용합니다. 예시로 `AbilityTask_MoveToLocation.h/.cpp`를 참조하세요.

**[⬆ 목차로 돌아가기](#table-of-contents)**

<a name="concepts-at-using"></a>
### 4.7.3 Using Ability Tasks
To create and activate an `AbilityTask` in C++ (From `GDGA_FireGun.cpp`):
```c++
UGDAT_PlayMontageAndWaitForEvent* Task = UGDAT_PlayMontageAndWaitForEvent::PlayMontageAndWaitForEvent(this, NAME_None, MontageToPlay, FGameplayTagContainer(), 1.0f, NAME_None, false, 1.0f);
Task->OnBlendOut.AddDynamic(this, &UGDGA_FireGun::OnCompleted);
Task->OnCompleted.AddDynamic(this, &UGDGA_FireGun::OnCompleted);
Task->OnInterrupted.AddDynamic(this, &UGDGA_FireGun::OnCancelled);
Task->OnCancelled.AddDynamic(this, &UGDGA_FireGun::OnCancelled);
Task->EventReceived.AddDynamic(this, &UGDGA_FireGun::EventReceived);
Task->ReadyForActivation();
```

In Blueprint, we just use the Blueprint node that we create for the `AbilityTask`. We don't have to call `ReadyForActivation()`. That is automatically called by `Engine/Source/Editor/GameplayTasksEditor/Private/K2Node_LatentGameplayTaskCall.cpp`. `K2Node_LatentGameplayTaskCall` also automatically calls `BeginSpawningActor()` and `FinishSpawningActor()` if they exist in your `AbilityTask` class (see `AbilityTask_WaitTargetData`). To reiterate, `K2Node_LatentGameplayTaskCall` only does automagic sorcery for Blueprint. In C++, we have to manually call `ReadyForActivation()`, `BeginSpawningActor()`, and `FinishSpawningActor()`.

![Blueprint WaitTargetData AbilityTask](https://github.com/tranek/GASDocumentation/raw/master/Images/abilitytask.png)

To manually cancel an `AbilityTask`, just call `EndTask()` on the `AbilityTask` object in Blueprint (called `Async Task Proxy`) or in C++.

**[⬆ Back to Top](#table-of-contents)**

<a name="concepts-at-rms"></a>
### 4.7.4 Root Motion Source Ability Tasks
GAS comes with `AbilityTasks` for moving `Characters` over time for things like knockbacks, complex jumps, pulls, and dashes using `Root Motion Sources` hooked into the `CharacterMovementComponent`.

**Note:** Predicting `RootMotionSource` `AbilityTasks` works up to engine version 4.19 and 4.25+. Prediction is bugged for engine versions 4.20-4.24; however, the `AbilityTasks` still perform their function in multiplayer with minor net corrections and work perfectly in single player. It is possible to cherry pick the [prediction fix](https://github.com/EpicGames/UnrealEngine/commit/94107438dd9f490e7b743f8e13da46927051bf33#diff-65f6196f9f28f560f95bd578e07e290c) from 4.25 into a custom 4.20-4.24 engine.

**[⬆ Back to Top](#table-of-contents)**

<a name="concepts-gc"></a>
### 4.8 Gameplay Cues

<a name="concepts-gc-definition"></a>
#### 4.8.1 Gameplay Cue Definition
`GameplayCues` (`GC`) execute non-gameplay related things like sound effects, particle effects, camera shakes, etc. `GameplayCues` are typically replicated (unless explicitly `Executed`, `Added`, or `Removed` locally) and predicted.

We trigger `GameplayCues` by sending a corresponding `GameplayTag` with the **mandatory parent name of `GameplayCue.`** and an event type (`Execute`, `Add`, or `Remove`) to the `GameplayCueManager` via the `ASC`. `GameplayCueNotify` objects and other `Actors` that implement the `IGameplayCueInterface` can subscribe to these events based on the `GameplayCue's` `GameplayTag` (`GameplayCueTag`).

**Note:** Just to reiterate, `GameplayCue` `GameplayTags` need to start with the parent `GameplayTag` of `GameplayCue`. So for example, a valid `GameplayCue` `GameplayTag` might be `GameplayCue.A.B.C`.

There are two classes of `GameplayCueNotifies`, `Static` and `Actor`. They respond to different events and different types of `GameplayEffects` can trigger them. Override the corresponding event with your logic.

| `GameplayCue` Class                                                                                                                  | Event             | `GameplayEffect` Type    | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| ------------------------------------------------------------------------------------------------------------------------------------ | ----------------- | ------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [`GameplayCueNotify_Static`](https://docs.unrealengine.com/en-US/API/Plugins/GameplayAbilities/UGameplayCueNotify_Static/index.html) | `Execute`         | `Instant` or `Periodic`  | Static `GameplayCueNotifies` operate on the `ClassDefaultObject` (meaning no instances) and are perfect for one-off effects like hit impacts.                                                                                                                                                                                                                                                                                                                                                                        |
| [`GameplayCueNotify_Actor`](https://docs.unrealengine.com/en-US/BlueprintAPI/GameplayCueNotify/index.html)                           | `Add` or `Remove` | `Duration` or `Infinite` | Actor `GameplayCueNotifies` spawn a new instance when `Added`. Because these are instanced, they can do actions over time until they are `Removed`. These are good for looping sounds and particle effects that will be removed when the backing `Duration` or `Infinite` `GameplayEffect` is removed or by manually calling remove. These also come with options to manage how many are allowed to be `Added` at the same time so that multiple applications of the same effect only start the sounds or particles once. |

`GameplayCueNotifies` technically can respond to any of the events but this is typically how we use them.

**Note:** When using `GameplayCueNotify_Actor`, check `Auto Destroy on Remove` otherwise subsequent calls to `Add` that `GameplayCueTag` won't work.

When using an `ASC` [Replication Mode](#concepts-asc-rm) other than `Full`, `Add` and `Remove` `GC` events will fire twice on Server players (listen server) - once for applying the `GE` and again from the "Minimal" `NetMultiCast` to the clients. However, `WhileActive` events will still only fire once. All events will only fire once on clients.

The Sample Project includes a `GameplayCueNotify_Actor` for stun and sprint effects. It also has a `GameplayCueNotify_Static` for the FireGun's projectile impact. These `GCs` can be optimized further by [triggering them locally](#concepts-gc-local) instead of replicating them through a `GE`. I opted for showing the beginner way of using them in the Sample Project.

**[⬆ Back to Top](#table-of-contents)**

<a name="concepts-gc-trigger"></a>
#### 4.8.2 Triggering Gameplay Cues

From inside of a `GameplayEffect` when it is successfully applied (not blocked by tags or immunity), fill in the `GameplayTags` of all the `GameplayCues` that should be triggered.

![GameplayCue Triggered from a GameplayEffect](https://github.com/tranek/GASDocumentation/raw/master/Images/gcfromge.png)

`UGameplayAbility` offers Blueprint nodes to `Execute`, `Add`, or `Remove` `GameplayCues`.

![GameplayCue Triggered from a GameplayAbility](https://github.com/tranek/GASDocumentation/raw/master/Images/gcfromga.png)

In C++, you can call functions directly on the `ASC` (or expose them to Blueprint in your `ASC` subclass):

```c++
/** GameplayCues can also come on their own. These take an optional effect context to pass through hit result, etc */
void ExecuteGameplayCue(const FGameplayTag GameplayCueTag, FGameplayEffectContextHandle EffectContext = FGameplayEffectContextHandle());
void ExecuteGameplayCue(const FGameplayTag GameplayCueTag, const FGameplayCueParameters& GameplayCueParameters);

/** Add a persistent gameplay cue */
void AddGameplayCue(const FGameplayTag GameplayCueTag, FGameplayEffectContextHandle EffectContext = FGameplayEffectContextHandle());
void AddGameplayCue(const FGameplayTag GameplayCueTag, const FGameplayCueParameters& GameplayCueParameters);

/** Remove a persistent gameplay cue */
void RemoveGameplayCue(const FGameplayTag GameplayCueTag);
	
/** Removes any GameplayCue added on its own, i.e. not as part of a GameplayEffect. */
void RemoveAllGameplayCues();
```

**[⬆ Back to Top](#table-of-contents)**

<a name="concepts-gc-local"></a>
#### 4.8.3 Local Gameplay Cues
The exposed functions for firing `GameplayCues` from `GameplayAbilities` and the `ASC` are replicated by default. Each `GameplayCue` event is a multicast RPC. This can cause a lot of RPCs. GAS also enforces a maximum of two of the same `GameplayCue` RPCs per net update. We avoid this by using local `GameplayCues` where we can. Local `GameplayCues` only `Execute`, `Add`, or `Remove` on the individual client.

Scenarios where we can use local `GameplayCues`:
* Projectile impacts
* Melee collision impacts
* `GameplayCues` fired from animation montages

Local `GameplayCue` functions that you should add to your `ASC` subclass:

```c++
UFUNCTION(BlueprintCallable, Category = "GameplayCue", Meta = (AutoCreateRefTerm = "GameplayCueParameters", GameplayTagFilter = "GameplayCue"))
void ExecuteGameplayCueLocal(const FGameplayTag GameplayCueTag, const FGameplayCueParameters& GameplayCueParameters);

UFUNCTION(BlueprintCallable, Category = "GameplayCue", Meta = (AutoCreateRefTerm = "GameplayCueParameters", GameplayTagFilter = "GameplayCue"))
void AddGameplayCueLocal(const FGameplayTag GameplayCueTag, const FGameplayCueParameters& GameplayCueParameters);

UFUNCTION(BlueprintCallable, Category = "GameplayCue", Meta = (AutoCreateRefTerm = "GameplayCueParameters", GameplayTagFilter = "GameplayCue"))
void RemoveGameplayCueLocal(const FGameplayTag GameplayCueTag, const FGameplayCueParameters& GameplayCueParameters);
```

```c++
void UPAAbilitySystemComponent::ExecuteGameplayCueLocal(const FGameplayTag GameplayCueTag, const FGameplayCueParameters & GameplayCueParameters)
{
	UAbilitySystemGlobals::Get().GetGameplayCueManager()->HandleGameplayCue(GetOwner(), GameplayCueTag, EGameplayCueEvent::Type::Executed, GameplayCueParameters);
}

void UPAAbilitySystemComponent::AddGameplayCueLocal(const FGameplayTag GameplayCueTag, const FGameplayCueParameters & GameplayCueParameters)
{
	UAbilitySystemGlobals::Get().GetGameplayCueManager()->HandleGameplayCue(GetOwner(), GameplayCueTag, EGameplayCueEvent::Type::OnActive, GameplayCueParameters);
	UAbilitySystemGlobals::Get().GetGameplayCueManager()->HandleGameplayCue(GetOwner(), GameplayCueTag, EGameplayCueEvent::Type::WhileActive, GameplayCueParameters);
}

void UPAAbilitySystemComponent::RemoveGameplayCueLocal(const FGameplayTag GameplayCueTag, const FGameplayCueParameters & GameplayCueParameters)
{
	UAbilitySystemGlobals::Get().GetGameplayCueManager()->HandleGameplayCue(GetOwner(), GameplayCueTag, EGameplayCueEvent::Type::Removed, GameplayCueParameters);
}
```

If a `GameplayCue` was `Added` locally, it should be `Removed` locally. If it was `Added` via replication, it should be `Removed` via replication.

**[⬆ Back to Top](#table-of-contents)**

<a name="concepts-gc-parameters"></a>
#### 4.8.4 Gameplay Cue Parameters
`GameplayCues` receive a `FGameplayCueParameters` structure containing extra information for the `GameplayCue` as a parameter. If you manually trigger the `GameplayCue` from a function on the `GameplayAbility` or the `ASC`, then you must manually fill in the `GameplayCueParameters` structure that is passed to the `GameplayCue`. If the `GameplayCue` is triggered by a `GameplayEffect`, then the following variables are automatically filled in on the `GameplayCueParameters` structure:

* AggregatedSourceTags
* AggregatedTargetTags
* GameplayEffectLevel
* AbilityLevel
* [EffectContext](#concepts-ge-context)
* Magnitude (if the `GameplayEffect` has an `Attribute` for magnitude selected in the dropdown above the `GameplayCue` tag container and a corresponding `Modifier` that affects that `Attribute`)

The `SourceObject` variable in the `GameplayCueParameters` structure is potentially a good place to pass arbitrary data to the `GameplayCue` when triggering the `GameplayCue` manually.

**Note:** Some of the variables in the parameters structure like `Instigator` might already exist in the `EffectContext`. The `EffectContext` can also contain a `FHitResult` for location of where to spawn the `GameplayCue` in the world. Subclassing `EffectContext` is potentially a good way to pass more data to `GameplayCues`, especially those triggered by a `GameplayEffect`.

See the 3 functions in [`UAbilitySystemGlobals`](#concepts-asg) that populate the `GameplayCueParameters` structure for more information. They are virtual so you can override them to autopopulate more information.

```c++
/** Initialize GameplayCue Parameters */
virtual void InitGameplayCueParameters(FGameplayCueParameters& CueParameters, const FGameplayEffectSpecForRPC &Spec);
virtual void InitGameplayCueParameters_GESpec(FGameplayCueParameters& CueParameters, const FGameplayEffectSpec &Spec);
virtual void InitGameplayCueParameters(FGameplayCueParameters& CueParameters, const FGameplayEffectContextHandle& EffectContext);
```

**[⬆ Back to Top](#table-of-contents)**

<a name="concepts-gc-manager"></a>
#### 4.8.5 Gameplay Cue Manager
By default, the `GameplayCueManager` will scan the entire game directory for `GameplayCueNotifies` and load them into memory on play. We can change the path where the `GameplayCueManager` scans by setting it in the `DefaultGame.ini`.

```
[/Script/GameplayAbilities.AbilitySystemGlobals]
GameplayCueNotifyPaths="/Game/GASDocumentation/Characters"
```

We do want the `GameplayCueManager` to scan and find all of the `GameplayCueNotifies`; however, we don't want it to async load every single one on play. This will put every `GameplayCueNotify` and all of their referenced sounds and particles into memory regardless if they're even used in a level. In a large game like Paragon, this can be hundreds of megabytes of unneeded assets in memory and cause hitching and game freezes on startup.

An alternative to async loading every `GameplayCue` on startup is to only async load `GameplayCues` as they're triggered in-game. This mitigates the unnecessary memory usage and potential game hard freezes while async loading every `GameplayCue` in exchange for potentially delayed effects for the first time that a specific `GameplayCue` is triggered during play. This potential delay is nonexistent for SSDs. I have not tested on a HDD. If using this option in the UE Editor, there may be slight hitches or freezes during the first load of GameplayCues if the Editor needs to compile particle systems. This is not an issue in builds as the particle systems will already be compiled.

First we must subclass `UGameplayCueManager` and tell the `AbilitySystemGlobals` class to use our `UGameplayCueManager` subclass in `DefaultGame.ini`.

```
[/Script/GameplayAbilities.AbilitySystemGlobals]
GlobalGameplayCueManagerClass="/Script/ParagonAssets.PBGameplayCueManager"
```

In our `UGameplayCueManager` subclass, override `ShouldAsyncLoadRuntimeObjectLibraries()`.

```c++
virtual bool ShouldAsyncLoadRuntimeObjectLibraries() const override
{
	return false;
}
```

**[⬆ Back to Top](#table-of-contents)**

<a name="concepts-gc-prevention"></a>
#### 4.8.6 Prevent Gameplay Cues from Firing
Sometimes we don't want `GameplayCues` to fire. For example if we block an attack, we may not want to play the hit impact attached to the damage `GameplayEffect` or play a custom one instead. We can do this inside of [`GameplayEffectExecutionCalculations`](#concepts-ge-ec) by calling `OutExecutionOutput.MarkGameplayCuesHandledManually()` and then manually sending our `GameplayCue` event to the `Target` or `Source's` `ASC`.

If you never want any `GameplayCues` to fire on a specific `ASC`, you can set `AbilitySystemComponent->bSuppressGameplayCues = true;`.

**[⬆ Back to Top](#table-of-contents)**

<a name="concepts-gc-batching"></a>
#### 4.8.7 Gameplay Cue Batching
Each `GameplayCue` triggered is an unreliable NetMulticast RPC. In situations where we fire multiple `GCs` at the same time, there are a few optimization methods to condense them down into one RPC or save bandwidth by sending less data.

<a name="concepts-gc-batching-manualrpc"></a>
##### 4.8.7.1 Manual RPC
Say you have a shotgun that shoots eight pellets. That's eight trace and impact `GameplayCues`. [GASShooter](https://github.com/tranek/GASShooter) takes the lazy approach of combining them into one RPC by stashing all of the trace information into the [`EffectContext`](#concepts-ge-ec) as [`TargetData`](#concepts-targeting-data). While this reduces the RPCs from eight to one, it still sends a lot of data over the network in that one RPC (~500 bytes). A more optimized approach is to send an RPC with a custom struct where you efficiently encode the hit locations or maybe you give it a random seed number to recreate/approximate the impact locations on the receiving side. The clients would then unpack this custom struct and turn back into [locally executed `GameplayCues`](#concepts-gc-local).

How this works:
1. Declare a `FScopedGameplayCueSendContext`. This suppresses `UGameplayCueManager::FlushPendingCues()` until it falls out of scope, meaning all `GameplayCues` will be queued up until the `FScopedGameplayCueSendContext` falls out of scope.
1. Override `UGameplayCueManager::FlushPendingCues()` to merge `GameplayCues` that can be batched together based on some custom `GameplayTag` into your custom struct and RPC it to clients.
1. Clients receive the custom struct and unpack it into locally executed `GameplayCues`.

This method can also be used when you need specific parameters for your `GameplayCues` that don't fit with what `GameplayCueParameters` offer and you don't want to add them to the `EffectContext` like damage numbers, crit indicator, broken shield indicator, was fatal hit indicator, etc.

https://forums.unrealengine.com/development-discussion/c-gameplay-programming/1711546-fscopedgameplaycuesendcontext-gameplaycuemanager

<a name="concepts-gc-batching-gcsonge"></a>
##### 4.8.7.2 Multiple GCs on one GE
All of the `GameplayCues` on a `GameplayEffect` are sent in one RPC already. By default, `UGameplayCueManager::InvokeGameplayCueAddedAndWhileActive_FromSpec()` will send the whole `GameplayEffectSpec` (but converted to `FGameplayEffectSpecForRPC`) in the unreliable NetMulticast regardless of the `ASC`'s `Replication Mode`. This could potentially be a lot of bandwidth depending on what is in the `GameplayEffectSpec`. We can potentially optimize this by setting the cvar `AbilitySystem.AlwaysConvertGESpecToGCParams 1`. This will convert `GameplayEffectSpecs` to `FGameplayCueParameter` structures and RPC those instead of the whole `FGameplayEffectSpecForRPC`. This potentially saves bandwidth but also has less information, depending on how the `GESpec` is converted to `GameplayCueParameters` and what your `GCs` need to know.

**[⬆ Back to Top](#table-of-contents)**

<a name="concepts-gc-events"></a>
#### 4.8.8 Gameplay Cue Events
`GameplayCues` respond to specific `EGameplayCueEvents`:

| `EGameplayCueEvent` | Description                                                                                                                                                                                                                                                                                                                         |
| ------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `OnActive`          | Called when a `GameplayCue` is activated (added).                                                                                                                                                                                                                                                                                   |
| `WhileActive`       | Called when `GameplayCue` is active, even if it wasn't actually just applied (Join in progress, etc). This is not `Tick`! It's called once just like `OnActive` when a `GameplayCueNotify_Actor` is added or becomes relevant. If you need `Tick()`, just use the `GameplayCueNotify_Actor`'s `Tick()`. It's an `AActor` after all. |
| `Removed`           | Called when a `GameplayCue` is removed. The Blueprint `GameplayCue` function that responds to this event is `OnRemove`.                                                                                                                                                                                                             |
| `Executed`          | Called when a `GameplayCue` is executed: instant effects or periodic `Tick()`. The Blueprint `GameplayCue` function that responds to this event is `OnExecute`.                                                                                                                                                                     |

Use `OnActive` for anything in your `GameplayCue` that happen at the start of the `GameplayCue` but is okay if late joiners miss. Use `WhileActive` for ongoing effects in the `GameplayCue` that you would want late joiners to see. For example, if you have a `GameplayCue` for a tower structure in a MOBA exploding, you would put the initial explosion particle system and explosion sound in `OnActive` and you would put any residual ongoing fire particles or sounds in the `WhileActive`. In this scenario, it wouldn't make sense for late joiners to replay the initial explosion from `OnActive`, but you would want them to see the persistent, looping fire effects on the ground after the explosion happened from `WhileActive`. `OnRemove` should clean up anything added in `OnActive` and `WhileActive`. `WhileActive` will be called every time an Actor enters the relevancy range of a `GameplayCueNotify_Actor`. `OnRemove` will be called every time an Actor leaves relevancy range of a `GameplayCueNotify_Actor`.

**[⬆ Back to Top](#table-of-contents)**

<a name="concepts-gc-reliability"></a>
#### 4.8.9 Gameplay Cue Reliability

`GameplayCues` in general should be considered unreliable and thus unsuited for anything that directly affects gameplay.

**Executed `GameplayCues`:** These `GameplayCues` are applied via unreliable multicasts and are always unreliable.

**`GameplayCues` applied from `GameplayEffects`:**
* Autonomous proxy reliably receives `OnActive`, `WhileActive`, and `OnRemove`  
`FActiveGameplayEffectsContainer::NetDeltaSerialize()` calls `UAbilitySystemComponent::HandleDeferredGameplayCues()` to call `OnActive` and `WhileActive`. `FActiveGameplayEffectsContainer::RemoveActiveGameplayEffectGrantedTagsAndModifiers()` makes the call to `OnRemoved`.
* Simulated proxies reliably receive `WhileActive` and `OnRemove`  
`UAbilitySystemComponent::MinimalReplicationGameplayCues`'s replication calls `WhileActive` and `OnRemove`. The `OnActive` event is called by an unreliable multicast.

**`GameplayCues` applied without a `GameplayEffect`:**
* Autonomous proxy reliably receives `OnRemove`  
The `OnActive` and `WhileActive` events are called by an unreliable multicast.
* Simulated proxies reliably receive `WhileActive` and `OnRemove`  
`UAbilitySystemComponent::MinimalReplicationGameplayCues`'s replication calls `WhileActive` and `OnRemove`. The `OnActive` event is called by an unreliable multicast.

If you need something in a `GameplayCue` to be 'reliable', then apply it from a `GameplayEffect` and use `WhileActive` to add the FX and `OnRemove` to remove the FX.

**[⬆ Back to Top](#table-of-contents)**

<a name="concepts-asg"></a>
### 4.9 Ability System Globals
The [`AbilitySystemGlobals`](https://docs.unrealengine.com/en-US/API/Plugins/GameplayAbilities/UAbilitySystemGlobals/index.html) class holds global information about GAS. Most of the variables can be set from the `DefaultGame.ini`. Generally you won't have to interact with this class, but you should be aware of its existence. If you need to subclass things like the [`GameplayCueManager`](#concepts-gc-manager) or the [`GameplayEffectContext`](#concepts-ge-context), you have to do that through the `AbilitySystemGlobals`.

To subclass `AbilitySystemGlobals`, set the class name in the `DefaultGame.ini`:
```
[/Script/GameplayAbilities.AbilitySystemGlobals]
AbilitySystemGlobalsClassName="/Script/ParagonAssets.PAAbilitySystemGlobals"
```

<a name="concepts-asg-initglobaldata"></a>
#### 4.9.1 InitGlobalData()
Between UE 4.24 and 5.2, it is necessary to call `UAbilitySystemGlobals::Get().InitGlobalData()` to use [`TargetData`](#concepts-targeting-data), otherwise you will get errors related to `ScriptStructCache` and clients will be disconnected from the server. This function only needs to be called once in a project. Fortnite calls it from `UAssetManager::StartInitialLoading()` and Paragon called it from `UEngine::Init()`. I find that putting it in `UAssetManager::StartInitialLoading()` is a good place as shown in the Sample Project. I would consider this boilerplate code that you should copy into your project to avoid issues with `TargetData`. Starting in 5.3 it is called automatically.

If you run into a crash while using the `AbilitySystemGlobals` `GlobalAttributeSetDefaultsTableNames`, you may need to call `UAbilitySystemGlobals::Get().InitGlobalData()` later like Fortnite in the `AssetManager` or in the `GameInstance`.

**[⬆ Back to Top](#table-of-contents)**

<a name="concepts-p"></a>
### 4.10 Prediction
GAS comes out of the box with support for client-side prediction; however, it does not predict everything. Client-side prediction in GAS means that the client does not have to wait for the server's permission to activate a `GameplayAbility` and apply `GameplayEffects`. It can "predict" the server giving it permission to do this and predict the targets that it would apply `GameplayEffects` to. The server then runs the `GameplayAbility` network latency-time after the client activates and tells the client if he was correct or not in his predictions. If the client was wrong in any of his predictions, he will "roll back" his changes from his "mispredictions" to match the server.

The definitive source for GAS-related prediction is `GameplayPrediction.h` in the plugin source code.

Epic's mindset is to only predict what you "can get away with". For example, Paragon and Fortnite do not predict damage. Most likely they use [`ExecutionCalculations`](#concepts-ge-ec) for their damage which cannot be predicted anyway. This is not to say that you can't try to predict certain things like damage. By all means if you do it and it works well for you then that's great.

> ... we are also not all in on a "predict everything: seamlessly and automatically" solution. We still feel player prediction is best kept to a minimum (meaning: predict the minimum amount of stuff you can get away with).

*Dave Ratti from Epic's comment from the new [Network Prediction Plugin](#concepts-p-npp)*

**What is predicted:**
> * Ability activation
> *	Triggered Events
> *	GameplayEffect application:
>    * Attribute modification (EXCEPTIONS: Executions do not currently predict, only attribute modifiers)
>    * GameplayTag modification
> * Gameplay Cue events (both from within predictive gameplay effect and on their own)
> * Montages
> * Movement (built into UE's UCharacterMovement)

**What is not predicted:**
> * GameplayEffect removal
> * GameplayEffect periodic effects (dots ticking)

*From `GameplayPrediction.h`*

While we can predict `GameplayEffect` application, we cannot predict `GameplayEffect` removal. One way that we can work around this limitation is to predict the inverse effect when we want to remove a `GameplayEffect`. Say we predict a movement speed slow of 40%. We can predictively remove it by applying a movement speed buff of 40%. Then remove both `GameplayEffects` at the same time. This is not appropriate for every scenario and support for predicting `GameplayEffect` removal is still needed. Dave Ratti from Epic has expressed desire to add it to a [future iteration of GAS](https://epicgames.ent.box.com/s/m1egifkxv3he3u3xezb9hzbgroxyhx89).

Because we cannot predict the removal of `GameplayEffects`, we cannot fully predict `GameplayAbility` cooldowns and there is no inverse `GameplayEffect` workaround for them. The server's replicated `Cooldown GE` will exist on the client and any attempts to bypass this (with `Minimal` replication mode for example) will be rejected by the server. This means clients with higher latencies take longer to tell the server to go on cooldown and to receive the removal of the server's `Cooldown GE`. This means players with higher latencies will have a lower rate of fire than players with lower latencies, giving them a disadvantage against lower latency players. Fortnite avoids this issue by using custom bookkeeping instead of `Cooldown GEs`.

Regarding predicting damage, I personally do not recommend it despite it being one of the first things that most people try when starting with GAS. I especially do not recommend trying to predict death. While you can predict damage, doing so is tricky. If you mispredict applying damage, the player will see the enemy's health jump back up. This can be especially awkward and frustrating if you try to predict death. Say you mispredict a `Character's` death and it starts ragdolling only to stop ragdolling and continue shooting at you when the server corrects it.

**Note:** `Instant`	`GameplayEffects` (like `Cost GEs`) that change `Attributes` can be predicted on yourself seamlessly, predicting `Instant` `Attribute` changes to other characters will show a brief anomaly or "blip" in their `Attributes`. Predicted `Instant` `GameplayEffects` are actually treated like `Infinite` `GameplayEffects` so that they can be rolled back if mispredicted. When the server's `GameplayEffect` is applied, there potentially exists two of the same `GameplayEffect's` causing the `Modifier` to be applied twice or not at all for a brief moment. It will eventually correct itself but sometimes the blip is noticeable to players.

Problems that GAS's prediction implementation is trying to solve:
> 1. "Can I do this?" Basic protocol for prediction.
> 2. "Undo" How to undo side effects when a prediction fails.
> 3. "Redo" How to avoid replaying side effects that we predicted locally but that also get replicated from the server.
> 4. "Completeness" How to be sure we /really/ predicted all side effects.
> 5. "Dependencies" How to manage dependent prediction and chains of predicted events.
> 6. "Override" How to override state predictively that is otherwise replicated/owned by the server.

*From `GameplayPrediction.h`*

**[⬆ Back to Top](#table-of-contents)**

<a name="concepts-p-key"></a>
#### 4.10.1 Prediction Key
GAS's prediction works on the concept of a `Prediction Key` which is an integer identifier that the client generates when he activates a `GameplayAbility`.

* Client generates a prediction key when it activates a `GameplayAbility`. This is the `Activation Prediction Key`.
* Client sends this prediction key to the server with `CallServerTryActivateAbility()`.
* Client adds this prediction key to all `GameplayEffects` that it applies while the prediction key is valid.
* Client's prediction key falls out of scope. Further predicted effects in the same `GameplayAbility` need a new [Scoped Prediction Window](#concepts-p-windows).


* Server receives the prediction key from the client.
* Server adds this prediction key to all `GameplayEffects` that it applies.
* Server replicates the prediction key back to the client.


* Client receives replicated `GameplayEffects` from the server with the prediction key used to apply them. If any of the replicated `GameplayEffects` match the `GameplayEffects` that the client applied with the same prediction key, they were predicted correctly. There will temporarily be two copies of the `GameplayEffect` on the target until the client removes its predicted one.
* Client receives the prediction key back from the server. This is the `Replicated Prediction Key`. This prediction key is now marked stale.
* Client removes **all** `GameplayEffects` that it created with the now stale replicated prediction key. `GameplayEffects` replicated by the server will persist. Any `GameplayEffects` that the client added and didn't receive a matching replicated version from the server were mispredicted.

Prediction keys are guaranteed to be valid during an atomic grouping of instructions "window" in `GameplayAbilities` starting with `Activation` from the activation prediction key. You can think of this as being only valid during one frame. Any callbacks from latent action `AbilityTasks` will no longer have a valid prediction key unless the `AbilityTask` has a built-in Synch Point which generates a new [Scoped Prediction Window](#concepts-p-windows).

**[⬆ Back to Top](#table-of-contents)**

<a name="concepts-p-windows"></a>
#### 4.10.2 Creating New Prediction Windows in Abilities
To predict more actions in callbacks from `AbilityTasks`, we need to create a new Scoped Prediction Window with a new Scoped Prediction Key. This is sometimes referred to as a Synch Point between the client and server. Some `AbilityTasks` like all of the input related ones come with built-in functionality to create a new scoped prediction window, meaning atomic code in the `AbilityTasks'` callbacks have a valid scoped prediction key to use. Other tasks like the `WaitDelay` task do not have built-in code to create a new scoped prediction window for its callback. If you need to predict actions after an `AbilityTask` that does not have built-in code to create a scoped prediction window like `WaitDelay`, we must manually do that using the `WaitNetSync` `AbilityTask` with the option `OnlyServerWait`. When the client hits a `WaitNetSync` with `OnlyServerWait`, it generates a new scoped prediction key based on the `GameplayAbility's` activation prediction key, RPCs it to the server, and adds it to any new `GameplayEffects` that it applies. When the server hits a `WaitNetSync` with `OnlyServerWait`, it waits until it receives the new scoped prediction key from the client before continuing. This scoped prediction key does the same dance as activation prediction keys - applied to `GameplayEffects` and replicated back to clients to be marked stale. The scoped prediction key is valid until it falls out of scope, meaning the scoped prediction window has closed. So again, only atomic operations, nothing latent, can use the new scoped prediction key.

You can create as many scoped prediction windows as you need.

If you would like to add the synch point functionality to your own custom `AbilityTasks`, look at how the input ones essentially inject the `WaitNetSync` `AbilityTask` code into them.

**Note:** When using `WaitNetSync`, this does block the server's `GameplayAbility` from continuing execution until it hears from the client. This could potentially be abused by malicious users who hack the game and intentionally delay sending their new scoped prediction key. While Epic uses the `WaitNetSync` sparingly, it recommends potentially building a new version of the `AbilityTask` with a delay that automatically continues without the client if this is a concern for you.

The Sample Project uses `WaitNetSync` in the Sprint `GameplayAbility` to create a new scoped prediction window every time we apply the stamina cost so that we can predict it. Ideally we want a valid prediction key when applying costs and cooldowns.

If you have a predicted `GameplayEffect` that is playing twice on the owning client, your prediction key is stale and you're experiencing the "redo" problem. You can usually solve this by putting a `WaitNetSync` `AbilityTask` with `OnlyServerWait` right before you apply the `GameplayEffect` to create a new scoped prediction key.

**[⬆ Back to Top](#table-of-contents)**

<a name="concepts-p-spawn"></a>
#### 4.10.3 Predictively Spawning Actors
Spawning `Actors` predictively on clients is an advanced topic. GAS does not provide functionality to handle this out of the box (the `SpawnActor` `AbilityTask` only spawns the `Actor` on the server). The key concept is to spawn a replicated `Actor` on both the client and the server.

If the `Actor` is just cosmetic or doesn't serve any gameplay purpose, the simple solution is to override the `Actor's` `IsNetRelevantFor()` function to restrict the server from replicating to the owning client. The owning client would have his locally spawned version and the server and other clients would have the server's replicated version.
```c++
bool APAReplicatedActorExceptOwner::IsNetRelevantFor(const AActor * RealViewer, const AActor * ViewTarget, const FVector & SrcLocation) const
{
	return !IsOwnedBy(ViewTarget);
}
```

If the spawned `Actor` affects gameplay like a projectile that needs to predict damage, then you need advanced logic that is outside of the scope of this documentation. Look at how UnrealTournament predictively spawns projectiles on Epic Games' GitHub. They have a dummy projectile spawned only on the owning client that synchs up with the server's replicated projectile.

**[⬆ Back to Top](#table-of-contents)**

<a name="concepts-p-future"></a>
#### 4.10.4 Future of Prediction in GAS
`GameplayPrediction.h` states in the future they could potentially add functionality for predicting `GameplayEffect` removal and periodic `GameplayEffects`.

Dave Ratti from Epic has [expressed interest](https://epicgames.ent.box.com/s/m1egifkxv3he3u3xezb9hzbgroxyhx89) in fixing the `latency reconciliation` problem for predicting cooldowns, disadvantaging players with higher latencies versus players with lower latencies.

The new [`Network Prediction` plugin](#concepts-p-npp) by Epic is expected to be fully interoperable with the GAS like the `CharacterMovementComponent` *was* before it.

**[⬆ Back to Top](#table-of-contents)**

<a name="concepts-p-npp"></a>
#### 4.10.5 Network Prediction Plugin
Epic recently started an initiative to replace the `CharacterMovementComponent` with a new `Network Prediction` plugin. This plugin is still in its very early stages but is available to very early access on the Unreal Engine GitHub. It's too soon to tell which future version of the Engine that it will make its experimental beta debut in.

**[⬆ Back to Top](#table-of-contents)**

<a name="concepts-targeting"></a>
### 4.11 Targeting

<a name="concepts-targeting-data"></a>
#### 4.11.1 Target Data
[`FGameplayAbilityTargetData`](https://docs.unrealengine.com/en-US/API/Plugins/GameplayAbilities/Abilities/FGameplayAbilityTargetData/index.html) is a generic structure for targeting data meant to be passed across the network. `TargetData` will typically hold `AActor`/`UObject` references, `FHitResults`, and other generic location/direction/origin information. However, you can subclass it to put essentially anything that you want inside of them as a simple means to [pass data between the client and server in `GameplayAbilities`](#concepts-ga-data). The base struct `FGameplayAbilityTargetData` is not meant to be used directly but instead subclassed. `GAS` comes with a few subclassed `FGameplayAbilityTargetData` structs out of the box located in `GameplayAbilityTargetTypes.h`.

`TargetData` is typically produced by [`Target Actors`](#concepts-targeting-actors) or **created manually** and consumed by [`AbilityTasks`](#concepts-at) and [`GameplayEffects`](#concepts-ge) via the [`EffectContext`](#concepts-ge-context). As a result of being in the `EffectContext`, [`Executions`](#concepts-ge-ec), [`MMCs`](#concepts-ge-mmc), [`GameplayCues`](#concepts-gc), and the functions on the backend of the [`AttributeSet`](#concepts-as) can access the `TargetData`.

We don't typically pass around the `FGameplayAbilityTargetData` directly, instead we use a [`FGameplayAbilityTargetDataHandle`](https://docs.unrealengine.com/en-US/API/Plugins/GameplayAbilities/Abilities/FGameplayAbilityTargetDataHandle/index.html) which has an internal TArray of pointers to `FGameplayAbilityTargetData`. This intermediate struct provides support for polymorphism of the `TargetData`.

An example of inheritting from `FGameplayAbilityTargetData`:
```c++
USTRUCT(BlueprintType)
struct MYGAME_API FGameplayAbilityTargetData_CustomData : public FGameplayAbilityTargetData
{
    GENERATED_BODY()
public:

    FGameplayAbilityTargetData_CustomData()
    { }

    UPROPERTY()
    FName CoolName = NAME_None;

    UPROPERTY()
    FPredictionKey MyCoolPredictionKey;

    // This is required for all child structs of FGameplayAbilityTargetData
    virtual UScriptStruct* GetScriptStruct() const override
    {
    	return FGameplayAbilityTargetData_CustomData::StaticStruct();
    }

	// This is required for all child structs of FGameplayAbilityTargetData
    bool NetSerialize(FArchive& Ar, class UPackageMap* Map, bool& bOutSuccess)
    {
	    // The engine already defined NetSerialize for FName & FPredictionKey, thanks Epic!
        CoolName.NetSerialize(Ar, Map, bOutSuccess);
        MyCoolPredictionKey.NetSerialize(Ar, Map, bOutSuccess);
        bOutSuccess = true;
        return true;
    }
}

template<>
struct TStructOpsTypeTraits<FGameplayAbilityTargetData_CustomData> : public TStructOpsTypeTraitsBase2<FGameplayAbilityTargetData_CustomData>
{
	enum
	{
        WithNetSerializer = true // This is REQUIRED for FGameplayAbilityTargetDataHandle net serialization to work
	};
};
```
For adding the target data to a handle:
```c++
UFUNCTION(BlueprintPure)
FGameplayAbilityTargetDataHandle MakeTargetDataFromCustomName(const FName CustomName)
{
	// Create our target data type, 
	// Handle's automatically cleanup and delete this data when the handle is destructed, 
	// if you don't add this to a handle then be careful because this deals with memory management and memory leaks so its safe to just always add it to a handle at some point in the frame!
	FGameplayAbilityTargetData_CustomData* MyCustomData = new FGameplayAbilityTargetData_CustomData();
	// Setup the struct's information to use the inputted name and any other changes we may want to do
	MyCustomData->CoolName = CustomName;
	
	// Make our handle wrapper for Blueprint usage
	FGameplayAbilityTargetDataHandle Handle;
	// Add the target data to our handle
	Handle.Add(MyCustomData);
	// Output our handle to Blueprint
	return Handle
}
```

For getting values it requires doing type safety checking, because the only way to get values from the handle's target data is by using generic C/C++ casting for it which is *NOT* type safe which can cause object slicing and crashes. For type checking there are multiple ways of doing this(however you want honestly) two common ways are:
- Gameplay Tag(s): You can use a subclass hierarchy where you know that anytime a certain code architecture's functionality occurs, you can cast for the base parent type and get its gameplay tag(s) and then compare against those for casting for inherited classes.
- Script Struct & Static Structs: You can instead do direct class comparison(which can involve a lot of IF statements or making some template functions), below is an example of doing this but basically you can get the script struct from any `FGameplayAbilityTargetData`(this is a nice advantage of it being a `USTRUCT` and requiring any inherited classes to specify the struct type in `GetScriptStruct`) and compare if its the type you're looking for. Below is an example of using these functions for type checking:
```c++
UFUNCTION(BlueprintPure)
FName GetCoolNameFromTargetData(const FGameplayAbilityTargetDataHandle& Handle, const int Index)
{   
    // NOTE, there is two versions of this '::Get(int32 Index)' function; 
    // 1) const version that returns 'const FGameplayAbilityTargetData*', good for reading target data values 
    // 2) non-const version that returns 'FGameplayAbilityTargetData*', good for modifying target data values
    FGameplayAbilityTargetData* Data = Handle.Get(Index); // This will valid check the index for you 
    
    // Valid check we have something to use, null data means nothing to cast for
    if(Data == nullptr)
    {
       	return NAME_None;
    }
    // This is basically the type checking pass, static_cast does not have type safety, this is why we do this check.
    // If we don't do this then it will object slice the struct and thus we have no way of making sure its that type.
    if(Data->GetScriptStruct() == FGameplayAbilityTargetData_CustomData::StaticStruct())
    {
        // Here is when you would do the cast because we know its the correct type already
        FGameplayAbilityTargetData_CustomData* CustomData = static_cast<FGameplayAbilityTargetData_CustomData*>(Data);    
        return CustomData->CoolName;
    }
    return NAME_None;
}
```

**[⬆ Back to Top](#table-of-contents)**

<a name="concepts-targeting-actors"></a>
#### 4.11.2 Target Actors
`GameplayAbilities` spawn [`TargetActors`](https://docs.unrealengine.com/en-US/API/Plugins/GameplayAbilities/Abilities/AGameplayAbilityTargetActor/index.html) with the `WaitTargetData` `AbilityTask` to visualize and capture targeting information from the world. `TargetActors` may optionally use [`GameplayAbilityWorldReticles`](#concepts-targeting-reticles) to display current targets. Upon confirmation, the targeting information is returned as [`TargetData`](#concepts-targeting-data) which can then be passed into `GameplayEffects`.
 
`TargetActors` are based on `AActor` so they can have any kind of visible component to represent **where** and **how** they are targeting such as static meshes or decals. Static meshes may be used to visualize placement of an object that your character will build. Decals may be used to show an area of effect on the ground. The Sample Project uses [`AGameplayAbilityTargetActor_GroundTrace`](https://docs.unrealengine.com/en-US/API/Plugins/GameplayAbilities/Abilities/AGameplayAbilityTargetActor_Grou-/index.html) with a decal on the ground to represent the damage area of effect for the Meteor ability. They also don't need to display anything either. For example it wouldn't make sense to display anything for a hitscan gun that instantly traces a line to its target as used in [GASShooter](https://github.com/tranek/GASShooter).

They capture targeting information using basic traces or collision overlaps and convert the results as `FHitResults` or `AActor` arrays to `TargetData` depending on the `TargetActor` implementation. The `WaitTargetData` `AbilityTask` determines when the targets are confirmed through its `TEnumAsByte<EGameplayTargetingConfirmation::Type> ConfirmationType` parameter. When **not** using `TEnumAsByte<EGameplayTargetingConfirmation::Type::Instant`, the `TargetActor` typically performs the trace/overlap on `Tick()` and updates its location to the `FHitResult` depending on its implementation. While this performs a trace/overlap on `Tick()`, it's generally not terrible since it's not replicated and you typically don't have more than one (although you could have more) `TargetActor` running at a time. Just be aware that it uses `Tick()` and some complex `TargetActors` might do a lot on it like the rocket launcher's secondary ability in GASShooter. While tracing on `Tick()` is very responsive to the client, you may consider lowering the tick rate on the `TargetActor` if the performance hit is too much. In the case of `TEnumAsByte<EGameplayTargetingConfirmation::Type::Instant`, the `TargetActor` immediately spawns, produces `TargetData`, and destroys. `Tick()` is never called. 

| `EGameplayTargetingConfirmation::Type` | When targets are confirmed                                                                                                                                                                                                                                                                                                                                     |
| -------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Instant`                              | The targeting happens instantly without special logic or user input deciding when to 'fire'.                                                                                                                                                                                                                                                                   |
| `UserConfirmed`                        | The targeting happens when the user confirms the targeting when the [ability is bound to a `Confirm` input](#concepts-ga-input) or by calling `UAbilitySystemComponent::TargetConfirm()`. The `TargetActor` will also respond to a bound `Cancel` input or call to `UAbilitySystemComponent::TargetCancel()` to cancel targeting.                              |
| `Custom`                               | The GameplayTargeting Ability is responsible for deciding when the targeting data is ready by calling `UGameplayAbility::ConfirmTaskByInstanceName()`. The `TargetActor` will also respond to `UGameplayAbility::CancelTaskByInstanceName()` to cancel targeting.                                                                                              |
| `CustomMulti`                          | The GameplayTargeting Ability is responsible for deciding when the targeting data is ready by calling `UGameplayAbility::ConfirmTaskByInstanceName()`. The `TargetActor` will also respond to `UGameplayAbility::CancelTaskByInstanceName()` to cancel targeting. Should not end the `AbilityTask` upon data production.                                       |

Not every EGameplayTargetingConfirmation::Type is supported by every `TargetActor`. For example, `AGameplayAbilityTargetActor_GroundTrace` does not support `Instant` confirmation.

The `WaitTargetData` `AbilityTask` takes in a `AGameplayAbilityTargetActor` class as a parameter and will spawn an instance on each activation of the `AbilityTask` and will destroy the `TargetActor` when the `AbilityTask` ends. The `WaitTargetDataUsingActor` `AbilityTask` takes in an already spawned `TargetActor`, but still destroys it when the `AbilityTask` ends. Both of these `AbilityTasks` are inefficient in that they either spawn or require a newly spawned `TargetActor` for each use. They're great for prototyping, but in production you might explore optimizing it if you have cases where you are constantly producing `TargetData` like in the case of an automatic rifle. GASShooter has a custom subclass of [`AGameplayAbilityTargetActor`](https://github.com/tranek/GASShooter/blob/master/Source/GASShooter/Public/Characters/Abilities/GSGATA_Trace.h) and a new [`WaitTargetDataWithReusableActor`](https://github.com/tranek/GASShooter/blob/master/Source/GASShooter/Public/Characters/Abilities/AbilityTasks/GSAT_WaitTargetDataUsingActor.h) `AbilityTask` written from scratch that allows you to reuse a `TargetActor` without destroying it.

`TargetActors` are not replicated by default; however, they can be made to replicate if that makes sense in your game to show other players where the local player is targeting. They do include default functionality to communicate with the server via RPCs on the `WaitTargetData` `AbilityTask`. If the `TargetActor`'s `ShouldProduceTargetDataOnServer` property is set to `false`, then the client will RPC its `TargetData` to the server on confirmation via `CallServerSetReplicatedTargetData()` in `UAbilityTask_WaitTargetData::OnTargetDataReadyCallback()`. If `ShouldProduceTargetDataOnServer` is `true`, the client will send a generic confirm event, `EAbilityGenericReplicatedEvent::GenericConfirm`, RPC to the server in `UAbilityTask_WaitTargetData::OnTargetDataReadyCallback()` and the server will do the trace or overlap check upon receiving the RPC to produce data on the server. If the client cancels the targeting, it will send a generic cancel event, `EAbilityGenericReplicatedEvent::GenericCancel`, RPC to the server in `UAbilityTask_WaitTargetData::OnTargetDataCancelledCallback`. As you can see, there are a lot of delegates on both the `TargetActor` and the `WaitTargetData` `AbilityTask`. The `TargetActor` responds to inputs to produce and broadcast `TargetData` ready, confirm, or cancel delegates. `WaitTargetData` listens to the `TargetActor`'s `TargetData` ready, confirm, and cancel delegates and relays that information back to the `GameplayAbility` and to the server. If you send `TargetData` to the server, you may want to do validation on the server to make sure the `TargetData` looks reasonable to prevent cheating. Producing the `TargetData` directly on the server avoids this issue entirely, but will potentially lead to mispredictions for the owning client.

Depending on the particular subclass of `AGameplayAbilityTargetActor` that you use, different `ExposeOnSpawn` parameters will be exposed on the `WaitTargetData` `AbilityTask` node. Some common parameters include:

| Common `TargetActor` Parameters | Definition                                                                                                                                                                                                                                                                                                               |
| ------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Debug                           | If `true`, it will draw debug tracing/overlapping information whenever the `TargetActor` performs a trace in non-shipping builds. Remember, non-`Instant` `TargetActors` will perform a trace on `Tick()` so these debug draw calls will also happen on `Tick()`.                                                        |
| Filter                          | [Optional] A special struct for filtering out (removing) `Actors` from the targets when the trace/overlap happens. Typical use cases are to filter out the player's `Pawn`, require targets be of a specific class. See [Target Data Filters](#concepts-target-data-filters) for more advanced use cases. |
| Reticle Class                   | [Optional] Subclass of `AGameplayAbilityWorldReticle` that the `TargetActor` will spawn.                                                                                                                                                                                                                                 |
| Reticle Parameters              | [Optional] Configure your Reticles. See [Reticles](#concepts-targeting-reticles).                                                                                                                                                                                                                                        |
| Start Location                  | A special struct for where tracing should start from. Typically this will be the player's viewpoint, a weapon muzzle, or the `Pawn`'s location.                                                                                                                                                                          |

With the default `TargetActor` classes, `Actors` are only valid targets when they are directly in the trace/overlap. If they leave the trace/overlap (they move or you look away), they are no longer valid. If you want the `TargetActor` to remember the last valid target(s), you will need to add this functionality to a custom `TargetActor` class. I refer to these as persistent targets as they will persist until the `TargetActor` receives confirmation or cancellation, the `TargetActor` finds a new valid target in its trace/overlap, or the target is no longer valid (destroyed). GASShooter uses persistent targets for its rocket launcher's secondary ability's homing rockets targeting.

**[⬆ Back to Top](#table-of-contents)**

<a name="concepts-target-data-filters"></a>
#### 4.11.3 Target Data Filters
Using both the `Make GameplayTargetDataFilter` and `Make Filter Handle` nodes, you can filter out the player's `Pawn` or select only a specific class. If you need more advanced filtering, you can subclass `FGameplayTargetDataFilter` and override the `FilterPassesForActor` function. 
```c++
USTRUCT(BlueprintType)
struct GASDOCUMENTATION_API FGDNameTargetDataFilter : public FGameplayTargetDataFilter
{
	GENERATED_BODY()

	/** Returns true if the actor passes the filter and will be targeted */
	virtual bool FilterPassesForActor(const AActor* ActorToBeFiltered) const override;
};
```

However, this will not work directly into the `Wait Target Data` node as it requires a `FGameplayTargetDataFilterHandle`. A new custom `Make Filter Handle` must be made to accept the subclass:
```c++
FGameplayTargetDataFilterHandle UGDTargetDataFilterBlueprintLibrary::MakeGDNameFilterHandle(FGDNameTargetDataFilter Filter, AActor* FilterActor)
{
	FGameplayTargetDataFilter* NewFilter = new FGDNameTargetDataFilter(Filter);
	NewFilter->InitializeFilterContext(FilterActor);

	FGameplayTargetDataFilterHandle FilterHandle;
	FilterHandle.Filter = TSharedPtr<FGameplayTargetDataFilter>(NewFilter);
	return FilterHandle;
}
```

**[⬆ Back to Top](#table-of-contents)**

<a name="concepts-targeting-reticles"></a>
#### 4.11.4 Gameplay Ability World Reticles
[`AGameplayAbilityWorldReticles`](https://docs.unrealengine.com/en-US/API/Plugins/GameplayAbilities/Abilities/AGameplayAbilityWorldReticle/index.html) (`Reticles`) visualize **who** you are targeting when targeting with non-`Instant` confirmed [`TargetActors`](#concepts-targeting-actors). `TargetActors` are responsible for the spawn and destroy lifetimes for all `Reticles`. `Reticles` are `AActors` so they can use any kind of visual component for representation. A common implementation as seen in [GASShooter](https://github.com/tranek/GASShooter) is to use a `WidgetComponent` to display a UMG Widget in screen space (always facing the player's camera). `Reticles` do not know which `AActor` that they're on, but you could subclass in that functionality on a custom `TargetActor`. `TargetActors` will typically update the `Reticle`'s location to the target's location on every `Tick()`.

GASShooter uses `Reticles` to show locked-on targets for the rocket launcher's secondary ability's homing rockets. The red indicator on the enemy is the `Reticle`. The similar white image is the rocket launcher's crosshair.
![Reticles in GASShooter](https://github.com/tranek/GASDocumentation/raw/master/Images/gameplayabilityworldreticle.png)

`Reticles` come with a handful of `BlueprintImplementableEvents` for designers (they're intended to be developed in Blueprints):

```c++
/** Called whenever bIsTargetValid changes value. */
UFUNCTION(BlueprintImplementableEvent, Category = Reticle)
void OnValidTargetChanged(bool bNewValue);

/** Called whenever bIsTargetAnActor changes value. */
UFUNCTION(BlueprintImplementableEvent, Category = Reticle)
void OnTargetingAnActor(bool bNewValue);

UFUNCTION(BlueprintImplementableEvent, Category = Reticle)
void OnParametersInitialized();

UFUNCTION(BlueprintImplementableEvent, Category = Reticle)
void SetReticleMaterialParamFloat(FName ParamName, float value);

UFUNCTION(BlueprintImplementableEvent, Category = Reticle)
void SetReticleMaterialParamVector(FName ParamName, FVector value);
```

`Reticles` can optionally use [`FWorldReticleParameters`](https://docs.unrealengine.com/en-US/API/Plugins/GameplayAbilities/Abilities/FWorldReticleParameters/index.html) provided by the `TargetActor` for configuration. The default struct only provides one variable `FVector AOEScale`. While you can technically subclass this struct, the `TargetActor` will only accept the base struct. It seems a little short-sighted to not allow this to be subclassed with default `TargetActors`. However, if you make your own custom `TargetActor`, you can provide your own custom reticle parameters struct and manually pass it to your subclass of `AGameplayAbilityWorldReticles` when you spawn them.

`Reticles` are not replicated by default, but can be made replicated if it makes sense for your game to show other players who the local player is targeting.

`Reticles` will only display on the current valid target with the default `TargetActors`. For example, if you're using a `AGameplayAbilityTargetActor_SingleLineTrace` to trace for a target, the `Reticle` will only appear when the enemy is directly in the trace path. If you look away, the enemy is no longer a valid target and the `Reticle` will disappear. If you want the `Reticle` to stay on the last valid target, you will want to customize your `TargetActor` to remember the last valid target and keep the `Reticle` on them. I refer to these as persistent targets as they will persist until the `TargetActor` receives confirmation or cancellation, the `TargetActor` finds a new valid target in its trace/overlap, or the target is no longer valid (destroyed).  GASShooter uses persistent targets for its rocket launcher's secondary ability's homing rockets targeting.

**[⬆ Back to Top](#table-of-contents)**

<a name="concepts-targeting-containers"></a>
#### 4.11.5 Gameplay Effect Containers Targeting
[`GameplayEffectContainers`](#concepts-ge-containers) come with an optional, efficient means of producing [`TargetData`](#concepts-targeting-data). This targeting takes place instantly when the `EffectContainer` is applied on the client and the server. It's more efficient than [`TargetActors`](#concepts-targeting-actors) because it runs on the CDO of the targeting object (no spawning and destroying of `Actors`), but it lacks player input, happens instantly without needing confirmation, cannot be canceled, and cannot send data from the client to the server (produces data on both). It works well for instant traces and collision overlaps. Epic's [Action RPG Sample Project](https://www.unrealengine.com/marketplace/en-US/product/action-rpg) includes two example types of targeting with its containers - target the ability owner and pull `TargetData` from an event. It also implements one in Blueprint to do instant sphere traces at some offset (set by child Blueprint classes) from the player. You can subclass `URPGTargetType` in C++ or Blueprint to make your own targeting types.

**[⬆ Back to Top](#table-of-contents)**

<a name="cae"></a>
## 5. Commonly Implemented Abilities and Effects

<a name="cae-stun"></a>
### 5.1 Stun
Typically with stuns, we want to cancel all of a `Character's` active `GameplayAbilities`, prevent new `GameplayAbility` activations, and prevent movement throughout the duration of the stun. The Sample Project's Meteor `GameplayAbility` applies a stun on hit targets.

To cancel the target's active `GameplayAbilities`, we call `AbilitySystemComponent->CancelAbilities()` when the stun [`GameplayTag` is added](#concepts-gt-change).

To prevent new `GameplayAbilities` from activating while stunned, the `GameplayAbilities` are given the stun `GameplayTag` in their [`Activation Blocked Tags` `GameplayTagContainer`](#concepts-ga-tags).

To prevent movement while stunned, we override the `CharacterMovementComponent's` `GetMaxSpeed()` function to return 0 when the owner has the stun `GameplayTag`.

**[⬆ Back to Top](#table-of-contents)**

<a name="cae-sprint"></a>
### 5.2 Sprint
The Sample Project provides an example of how to sprint - run faster while `Left Shift` is held down.

The faster movement is handled predictively by the `CharacterMovementComponent` by sending a flag over the network to the server. See `GDCharacterMovementComponent.h/cpp` for details.

The `GA` handles responding to the `Left Shift` input, tells the `CMC` to begin and stop sprinting, and to predictively charge stamina while `Left Shift` is pressed. See `GA_Sprint_BP` for details.

**[⬆ Back to Top](#table-of-contents)**

<a name="cae-ads"></a>
### 5.3 Aim Down Sights
The Sample Project handles this the exact same way as sprinting but decreasing the movement speed instead of increasing it.

See `GDCharacterMovementComponent.h/cpp` for details on predictively decreasing the movement speed.

See `GA_AimDownSight_BP` for details on handling the input. There is no stamina cost for aiming down sights.

**[⬆ Back to Top](#table-of-contents)**

<a name="cae-ls"></a>
### 5.4 Lifesteal
I handle lifesteal inside of the damage [`ExecutionCalculation`](#concepts-ge-ec). The `GameplayEffect` will have a `GameplayTag` on it like `Effect.CanLifesteal`. The `ExecutionCalculation` checks if the `GameplayEffectSpec` has that `Effect.CanLifesteal` `GameplayTag`. If the `GameplayTag` exists, the `ExecutionCalculation` [creates a dynamic `Instant` `GameplayEffect`](#concepts-ge-dynamic) with the amount of health to give as the modifier and applies it back to the `Source's` `ASC`.

```c++
if (SpecAssetTags.HasTag(FGameplayTag::RequestGameplayTag(FName("Effect.Damage.CanLifesteal"))))
{
	float Lifesteal = Damage * LifestealPercent;

	UGameplayEffect* GELifesteal = NewObject<UGameplayEffect>(GetTransientPackage(), FName(TEXT("Lifesteal")));
	GELifesteal->DurationPolicy = EGameplayEffectDurationType::Instant;

	int32 Idx = GELifesteal->Modifiers.Num();
	GELifesteal->Modifiers.SetNum(Idx + 1);
	FGameplayModifierInfo& Info = GELifesteal->Modifiers[Idx];
	Info.ModifierMagnitude = FScalableFloat(Lifesteal);
	Info.ModifierOp = EGameplayModOp::Additive;
	Info.Attribute = UPAAttributeSetBase::GetHealthAttribute();

	SourceAbilitySystemComponent->ApplyGameplayEffectToSelf(GELifesteal, 1.0f, SourceAbilitySystemComponent->MakeEffectContext());
}
```

**[⬆ Back to Top](#table-of-contents)**

<a name="cae-random"></a>
### 5.5 Generating a Random Number on Client and Server
Sometimes you need to generate a "random" number inside of a `GameplayAbility` for things like bullet recoil or spread. The client and the server will both want to generate the same random numbers. To do this, we must set the `random seed` to be the same at the time of `GameplayAbility` activation. You will want to set the `random seed` each time you activate the `GameplayAbility` in case the client mispredicts activation and its random number sequence becomes out of synch with the server's.

| Seed Setting Method                                                          | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| ---------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Use the activation prediction key                                            | The `GameplayAbility` activation prediction key is an int16 guaranteed to be synchronized and available in both the client and server in the `Activation()`. You can set this as the `random seed` on both the client and the server. The downside to this method is that the prediction key always starts at zero each time the game starts and consistently increments the value to use between generating keys. This means each match will have the exact same random number sequence. This may or may not be random enough for your needs. |
| Send a seed through an event payload when you activate the `GameplayAbility` | Activate your `GameplayAbility` by event and send the randomly generated seed from the client to the server via the replicated event payload. This allows for more randomness but the client could easily hack their game to only send the same seed value every time. Also activating `GameplayAbilities` by event will prevent them from activating from the input bind.                                                                                                                                                                     |

If your random deviation is small, most players won't notice that the sequence is the same every game and using the activation prediction key as the `random seed` should work for you. If you're doing something more complex that needs to be hacker proof, perhaps using a `Server Initiated` `GameplayAbility` would work better where the server can create the prediction key or generate the `random seed` to send via an event payload.

**[⬆ Back to Top](#table-of-contents)**

<a name="cae-crit"></a>
### 5.6 Critical Hits
I handle critical hits inside of the damage [`ExecutionCalculation`](#concepts-ge-ec). The `GameplayEffect` will have a `GameplayTag` on it like `Effect.CanCrit`. The `ExecutionCalculation` checks if the `GameplayEffectSpec` has that `Effect.CanCrit` `GameplayTag`. If the `GameplayTag` exists, the `ExecutionCalculation` generates a random number corresponding to the critical hit chance (`Attribute` captured from the `Source`) and adds the critical hit damage (also an `Attribute` captured from the `Source`) if it succeeded. Since I don't predict damage, I don't have to worry about synchronizing the random number generators on the client and server since the `ExecutionCalculation` will only run on the server. If you tried to do this predictively using an `MMC` to do your damage calculation, you would have to get a reference to the `random seed` from the `GameplayEffectSpec->GameplayEffectContext->GameplayAbilityInstance`.

See how [GASShooter](https://github.com/tranek/GASShooter) does headshots. It's the same concept except that it does not rely on a random number for chance and instead checks the `FHitResult` bone name.

**[⬆ Back to Top](#table-of-contents)**

<a name="cae-nonstackingge"></a>
### 5.7 Non-Stacking Gameplay Effects but Only the Greatest Magnitude Actually Affects the Target
Slow effects in Paragon did not stack. Each slow instance applied and kept track of their lifetimes as normal, but only the greatest magnitude slow effect actually affected the `Character`. GAS provides for this scenario out of the box with `AggregatorEvaluateMetaData`. See [`AggregatorEvaluateMetaData()`](#concepts-as-onattributeaggregatorcreated) for details and implementation.

**[⬆ Back to Top](#table-of-contents)**

<a name="cae-paused"></a>
### 5.8 Generate Target Data While Game is Paused
If you need to pause the game while waiting to generate [`TargetData`](#concepts-targeting-data) from a `WaitTargetData` `AbilityTask` from your player, I suggest instead of pausing to use `slomo 0`.

**[⬆ Back to Top](#table-of-contents)**

<a name="cae-onebuttoninteractionsystem"></a>
### 5.9 One Button Interaction System
[GASShooter](https://github.com/tranek/GASShooter) implements a one button interaction system where the player can press or hold 'E' to interact with interactable objects like reviving a player, opening a weapon chest, and opening or closing a sliding door.

**[⬆ Back to Top](#table-of-contents)**

<a name="debugging"></a>
## 6. Debugging GAS
Often when debugging GAS related issues, you want to know things like:
> * "What are the values of my attributes?"
> * "What gameplay tags do I have?"
> * "What gameplay effects do I currently have?"
> * "What abilities do I have granted, which ones are running, and which ones are blocked from activating?".

GAS comes with two techniques for answering these questions at runtime - [`showdebug abilitysystem`](#debugging-sd) and hooks in the [`GameplayDebugger`](#debugging-gd).

**Tip:** Unreal Engine likes to optimize C++ code which makes it hard to debug some functions. You will encounter this rarely when tracing deep into your code. If setting your Visual Studio solution configuration to `DebugGame Editor` still prevents tracing code or inspecting variables, you can disable all optimizations by wrapping the optimized function with the `UE_DISABLE_OPTIMIZATION` and `UE_ENABLE_OPTIMIZATION` macros or the ship variations defined in CoreMiscDefines.h. This cannot be used on the plugin code unless you rebuild the plugin from source. This may or may not work on inline functions depending on what they do and where they are. Be sure to remove the macros when you're done debugging!

```c++
UE_DISABLE_OPTIMIZATION
void MyClass::MyFunction(int32 MyIntParameter)
{
	// My code
}
UE_ENABLE_OPTIMIZATION
```

**[⬆ Back to Top](#table-of-contents)**

<a name="debugging-sd"></a>
### 6.1 showdebug abilitysystem
Type `showdebug abilitysystem` in the in-game console. This feature is split into three "pages". All three pages will show the `GameplayTags` that you currently have. Type `AbilitySystem.Debug.NextCategory` into the console to cycle between the pages.

The first page shows the `CurrentValue` of all of your `Attributes`:
![First Page of showdebug abilitysystem](https://github.com/tranek/GASDocumentation/raw/master/Images/showdebugpage1.png)

The second page shows all of the `Duration` and `Infinite` `GameplayEffects` on you, their number of stacks, what `GameplayTags` they give, and what `Modifiers` they give.
![Second Page of showdebug abilitysystem](https://github.com/tranek/GASDocumentation/raw/master/Images/showdebugpage2.png)

The third page shows all of the `GameplayAbilities` that have been granted to you, whether they are currently running, whether they are blocked from activating, and the status of currently running `AbilityTasks`.
![Third Page of showdebug abilitysystem](https://github.com/tranek/GASDocumentation/raw/master/Images/showdebugpage3.png)

To cycle between targets (denoted by a green rectangular prism around the Actor), use the `PageUp` key or `NextDebugTarget` console command to go to the next target and the `PageDown` key or `PreviousDebugTarget` console command to go to the previous target.

**Note:** In order for the ability system information to update based on the currently selected debug Actor, you need to set `bUseDebugTargetFromHud=true` in the `AbilitySystemGlobals` like so in the `DefaultGame.ini`:
```
[/Script/GameplayAbilities.AbilitySystemGlobals]
bUseDebugTargetFromHud=true
```

**Note:** For `showdebug abilitysystem` to work an actual HUD class must be selected in the GameMode. Otherwise the command is not found and "Unknown Command" is returned.

**[⬆ Back to Top](#table-of-contents)**

<a name="debugging-gd"></a>
### 6.2 Gameplay Debugger
GAS adds functionality to the Gameplay Debugger. Access the Gameplay Debugger with the Apostrophe (') key. Enable the Abilities category by pressing 3 on your numpad. The category may be different depending on what plugins you have. If your keyboard doesn't have a numpad like a laptop, then you can change the keybindings in the project settings.

Use the Gameplay Debugger when you want to see the `GameplayTags`, `GameplayEffects`, and `GameplayAbilities` on **other** `Characters`. Unfortunately it does not show the `CurrentValue` of the target's `Attributes`. It will target whatever `Character` is in the center of your screen. You can change targets by selecting them in the World Outliner in the Editor or by looking at a different `Character` and press Apostrophe (') again. The currently inspected `Character` has the largest red circle above it.

![Gameplay Debugger](https://github.com/tranek/GASDocumentation/raw/master/Images/gameplaydebugger.png)

**[⬆ Back to Top](#table-of-contents)**

<a name="debugging-log"></a>
### 6.3 GAS Logging
The GAS source code contains a lot of logging statements produced at varying verbosity levels. You will most likely see these as `ABILITY_LOG()` statements. The default verbosity level is `Display`. Anything higher will not be displayed in the console by default.

To change the verbosity level of a log category, type into your console:

```
log [category] [verbosity]
```

For example, to turn on `ABILITY_LOG()` statements, you would type into your console:
```
log LogAbilitySystem VeryVerbose
```

To reset it back to default, type:
```
log LogAbilitySystem Display
```

To display all log categories, type:
```
log list
```

Notable GAS related logging categories:

| Logging Category          | Default Verbosity Level |
| ------------------------- | ----------------------- |
| LogAbilitySystem          | Display                 |
| LogAbilitySystemComponent | Log                     |
| LogGameplayCueDetails     | Log                     |
| LogGameplayCueTranslator  | Display                 |
| LogGameplayEffectDetails  | Log                     |
| LogGameplayEffects        | Display                 |
| LogGameplayTags           | Log                     |
| LogGameplayTasks          | Log                     |
| VLogAbilitySystem         | Display                 |

See the [Wiki on Logging](https://unrealcommunity.wiki/logging-lgpidy6i) for more information.

**[⬆ Back to Top](#table-of-contents)**

<a name="optimizations"></a>
## 7. Optimizations

<a name="optimizations-abilitybatching"></a>
### 7.1 Ability Batching
[`GameplayAbilities`](#concepts-ga) that activate, optionally send `TargetData` to the server, and end all in one frame can be [batched to condense two-three RPCs into one RPC](#concepts-ga-batching). These types of abilities are commonly used for hitscan guns.

<a name="optimizations-gameplaycuebatching"></a>
### 7.2 Gameplay Cue Batching
If you're sending many [`GameplayCues`](#concepts-gc) at the same time, consider [batching them into one RPC](#concepts-gc-batching). The goal is to reduce the number of RPCs (`GameplayCues` are unreliable NetMulticasts) and send as little data as possible.

<a name="optimizations-ascreplicationmode"></a>
### 7.3 AbilitySystemComponent Replication Mode
By default, the [`ASC`](#concepts-asc) is in [`Full Replication Mode`](#concepts-asc-rm). This will replicate all [`GameplayEffects`](#concepts-ge) to every client (which is fine for a single player game). In a multiplayer game, set the player owned `ASCs` to `Mixed Replication Mode` and AI controlled characters to `Minimal Replication Mode`. This will replicate `GEs` applied on a player character to only replicate to the owner of that character and `GEs` applied on AI controlled characters will never replicate `GEs` to clients. [`GameplayTags`](#concepts-gt) will still replicate and [`GameplayCues`](#concepts-gc) will still be unreliable NetMulticast to all clients, regardless of the `Replication Mode`. This will cut down on network data from `GEs` being replicated when all clients don't need to see them.

<a name="optimizations-attributeproxyreplication"></a>
### 7.4 Attribute Proxy Replication
In large games with many players like Fortnite Battle Royale (FNBR), there will be a lot of [`ASCs`](#concepts-asc) living on always-relevant `PlayerStates` replicating a lot of [`Attributes`](#concepts-a). To optimize this bottleneck, Fortnite disables the `ASC` and its [`AttributeSets`](#concepts-as) from replicating altogether on **simulated player-controlled proxies** in the `PlayerState::ReplicateSubobjects()`. Autonomous proxies and AI controlled `Pawns` still fully replicate according to their [`Replication Mode`](#concepts-asc-rm). Instead of replicating `Attributes` on the `ASC` on the always-relevant `PlayerStates`, FNBR uses a replicated proxy structure on the player's `Pawn`. When `Attributes` change on the server's `ASC`, they are changed on the proxy struct too. The client receives the replicated `Attributes` from the proxy struct and pushes the changes back into its local `ASC`. This allows `Attribute` replication to use the `Pawn`'s relevancy and `NetUpdateFrequency`. This proxy struct also replicates a small white-listed set of `GameplayTags` in a bitmask. This optimization reduces the amount of data over the network and allows us to take advantage of pawn relevancy. AI controlled `Pawns` have their `ASC` on the `Pawn` which already uses its relevancy so this optimization is not needed for them.

> I’m not sure if it is still necessary with other server side optimizations that have been done since then (Replication Graph, etc) and it is not the most maintainable pattern.

*Dave Ratti from Epic's answer to [community questions #3](https://epicgames.ent.box.com/s/m1egifkxv3he3u3xezb9hzbgroxyhx89)*

<a name="optimizations-asclazyloading"></a>
### 7.5 ASC Lazy Loading
Fortnite Battle Royale (FNBR) has a lot of damageable `AActors` (trees, buildings, etc) in the world, each with an [`ASC`](#concepts-asc). This can add up in memory cost. FNBR optimizes this by lazily loading `ASCs` only when they're needed (when they first take damage by a player). This reduces overall memory usage since some `AActors` may never be damaged in a match.

**[⬆ Back to Top](#table-of-contents)**

<a name="qol"></a>
## 8. Quality of Life Suggestions

<a name="qol-gameplayeffectcontainers"></a>
### 8.1 Gameplay Effect Containers
[GameplayEffectContainers](#concepts-ge-containers) combine [`GameplayEffectSpecs`](#concepts-ge-spec), [`TargetData`](#concepts-targeting-data), [simple targeting](#concepts-targeting-containers), and related functionality into easy to use structures. These are great for transfering `GameplayEffectSpecs` to projectiles spawned from an ability that will then apply them on collision at a later time.

<a name="qol-asynctasksascdelegates"></a>
### 8.2 Blueprint AsyncTasks to Bind to ASC Delegates
To increase designer-friendly iteration times, especially when designing UMG Widgets for UI, create Blueprint AsyncTasks (in C++) to bind to the common change delegates on the `ASC` directly from your UMG Blueprint graphs. The only caveat is that they must be manually destroyed (like when the widget is destroyed) otherwise they will live in memory forever. The Sample Project includes three Blueprint AsyncTasks.

Listen for `Attribute` changes:

![Listen for Attributes Changes BP Node](https://github.com/tranek/GASDocumentation/raw/master/Images/attributeschange.png)

Listen for cooldown changes:

![Listen for Cooldown Change BP Node](https://github.com/tranek/GASDocumentation/raw/master/Images/cooldownchange.png)

Listen for `GE` stack changes:

![Listen for GameplayEffect Stack Change BP Node](https://github.com/tranek/GASDocumentation/raw/master/Images/gestackchange.png)

**[⬆ Back to Top](#table-of-contents)**

<a name="troubleshooting"></a>
## 9. Troubleshooting

<a name="troubleshooting-notlocal"></a>
### 9.1 `LogAbilitySystem: Warning: Can't activate LocalOnly or LocalPredicted ability %s when not local!`
You need to [initialize the `ASC` on the client](#concepts-asc-setup).

**[⬆ Back to Top](#table-of-contents)**

<a name="troubleshooting-scriptstructcache"></a>
### 9.2 `ScriptStructCache` errors
You need to call [`UAbilitySystemGlobals::InitGlobalData()`](#concepts-asg-initglobaldata).

**[⬆ Back to Top](#table-of-contents)**

<a name="troubleshooting-replicatinganimmontages"></a>
### 9.3 Animation Montages are not replicating to clients
Make sure that you're using the `PlayMontageAndWait` Blueprint node instead of `PlayMontage` in your [GameplayAbilities](#concepts-ga). This [AbilityTask](#concepts-at) replicates the montage through the `ASC` automatically whereas the `PlayMontage` node does not.

**[⬆ Back to Top](#table-of-contents)**

<a name="troubleshooting-duplicatingblueprintactors"></a>
### 9.4 Duplicating Blueprint Actors is setting AttributeSets to nullptr
There is a [bug in Unreal Engine](https://issues.unrealengine.com/issue/UE-81109) that will set `AttributeSet` pointers on your classes to nullptr for Blueprint Actor classes that are duplicated from existing Blueprint Actor classes. There are a few workarounds for this. I've had success not creating bespoke `AttributeSet` pointers on my classes (no pointer in the .h, not calling `CreateDefaultSubobject` in the constructor) and instead just directly adding `AttributeSets` to the `ASC` in `PostInitializeComponents()` (not shown in the Sample Project). The replicated `AttributeSets` will still live in the `ASC's` `SpawnedAttributes` array. It would look something like this:

```c++
void AGDPlayerState::PostInitializeComponents()
{
	Super::PostInitializeComponents();

	if (AbilitySystemComponent)
	{
		AbilitySystemComponent->AddSet<UGDAttributeSetBase>();
		// ... any other AttributeSets that you may have
	}
}
```

In this scenario, you would read and set the values in the `AttributeSet` using the functions on the `ASC` instead of [calling functions on the `AttributeSet` made from the macros](#concepts-as-attributes).

```c++
/** Returns current (final) value of an attribute */
float GetNumericAttribute(const FGameplayAttribute &Attribute) const;

/** Sets the base value of an attribute. Existing active modifiers are NOT cleared and will act upon the new base value. */
void SetNumericAttributeBase(const FGameplayAttribute &Attribute, float NewBaseValue);
```

So the `GetHealth()` would look something like:

```c++
float AGDPlayerState::GetHealth() const
{
	if (AbilitySystemComponent)
	{
		return AbilitySystemComponent->GetNumericAttribute(UGDAttributeSetBase::GetHealthAttribute());
	}

	return 0.0f;
}
```

Setting (initializing) the health `Attribute` would look something like:

```c++
const float NewHealth = 100.0f;
if (AbilitySystemComponent)
{
	AbilitySystemComponent->SetNumericAttributeBase(UGDAttributeSetBase::GetHealthAttribute(), NewHealth);
}
```

As a reminder, the `ASC` only ever expects at most one `AttributeSet` object per `AttributeSet` class.

**[⬆ Back to Top](#table-of-contents)**

<a name="troubleshooting-unresolvedexternalsymbolmarkpropertydirty"></a>
### 9.5 Unresolved external symbol UEPushModelPrivate::MarkPropertyDirty(int,int)

If you get a compiler error like:

```
error LNK2019: unresolved external symbol "__declspec(dllimport) void __cdecl UEPushModelPrivate::MarkPropertyDirty(int,int)" (__imp_?MarkPropertyDirty@UEPushModelPrivate@@YAXHH@Z) referenced in function "public: void __cdecl FFastArraySerializer::IncrementArrayReplicationKey(void)" (?IncrementArrayReplicationKey@FFastArraySerializer@@QEAAXXZ)
```

This is from trying to call `MarkItemDirty()` on a `FFastArraySerializer`. I've encountered this from updating an `ActiveGameplayEffect` such as when updating the cooldown duration.

```c++
ActiveGameplayEffects.MarkItemDirty(*AGE);
```

What's happening is that `WITH_PUSH_MODEL` is getting defined in more than one place. `PushModelMacros.h` is defining it as 0 while it's defined as 1 in multiple places. `PushModel.h` is seeing it as 1 but `PushModel.cpp` is seeing it as 0.

The solution is to add `NetCore` to your project's `PublicDependencyModuleNames` in the `Build.cs`.

**[⬆ Back to Top](#table-of-contents)**

<a name="troubleshooting-enumnamesarenowpathnames"></a>
### 9.6 Enum names are now represented by path name

If you get a compiler warning like:

```
warning C4996: 'FGameplayAbilityInputBinds::FGameplayAbilityInputBinds': Enum names are now represented by path names. Please use a version of FGameplayAbilityInputBinds constructor that accepts FTopLevelAssetPath. Please update your code to the new API before upgrading to the next release, otherwise your project will no longer compile.
```

UE 5.1 deprecated using `FString` in the constructor for `BindAbilityActivationToInputComponent()`. Instead, we must pass in an `FTopLevelAssetPath`.

Old, deprecated way:
```c++
AbilitySystemComponent->BindAbilityActivationToInputComponent(InputComponent, FGameplayAbilityInputBinds(FString("ConfirmTarget"),
	FString("CancelTarget"), FString("EGDAbilityInputID"), static_cast<int32>(EGDAbilityInputID::Confirm), static_cast<int32>(EGDAbilityInputID::Cancel)));
```

New way:
```c++
FTopLevelAssetPath AbilityEnumAssetPath = FTopLevelAssetPath(FName("/Script/GASDocumentation"), FName("EGDAbilityInputID"));
AbilitySystemComponent->BindAbilityActivationToInputComponent(InputComponent, FGameplayAbilityInputBinds(FString("ConfirmTarget"),
	FString("CancelTarget"), AbilityEnumAssetPath, static_cast<int32>(EGDAbilityInputID::Confirm), static_cast<int32>(EGDAbilityInputID::Cancel)));
```

See `Engine\Source\Runtime\CoreUObject\Public\UObject\TopLevelAssetPath.h` for more info.

**[⬆ Back to Top](#table-of-contents)**

<a name="acronyms"></a>
## 10. Common GAS Acronyms

| Name                                                                                                   | Acronyms            |
|------------------------------------------------------------------------------------------------------- | ------------------- |
| AbilitySystemComponent                                                                                 | ASC                 |
| AbilityTask                                                                                            | AT                  |
| [Action RPG Sample Project by Epic](https://www.unrealengine.com/marketplace/en-US/product/action-rpg) | ARPG, ARPG Sample   |
| CharacterMovementComponent                                                                             | CMC                 |
| GameplayAbility                                                                                        | GA                  |
| GameplayAbilitySystem                                                                                  | GAS                 |
| GameplayCue                                                                                            | GC                  |
| GameplayEffect                                                                                         | GE                  |
| GameplayEffectExecutionCalculation                                                                     | ExecCalc, Execution |
| GameplayTag                                                                                            | Tag, GT             |
| ModifierMagnitudeCalculation                                                                           | ModMagCalc, MMC     |

**[⬆ Back to Top](#table-of-contents)**

<a name="resources"></a>
## 11. Other Resources
* [Official Documentation](https://docs.unrealengine.com/en-US/Gameplay/GameplayAbilitySystem/index.html)
* Source Code!
   * Especially `GameplayPrediction.h`
* [Lyra Sample Project by Epic](https://unrealengine.com/marketplace/en-US/learn/lyra)
* [Action RPG Sample Project by Epic](https://www.unrealengine.com/marketplace/en-US/product/action-rpg)
* [Unreal Slackers Discord](https://unrealslackers.org/) has a text channel dedicated to GAS `#gameplay-ability-system`
   * Check pinned messages
* [GitHub repository of resources by Dan 'Pan'](https://github.com/Pantong51/GASContent)
* [YouTube Videos by SabreDartStudios](https://www.youtube.com/channel/UCCFUhQ6xQyjXDZ_d6X_H_-A)

<a name="resources-daveratti"></a>
### 11.1 Q&A With Epic Game's Dave Ratti

<a name="resources-daveratti-community1"></a>
#### 11.1.1 Community Questions 1
[Dave Ratti responses to the Unreal Slackers Discord Server community questions about GAS](https://epicgames.ent.box.com/s/m1egifkxv3he3u3xezb9hzbgroxyhx89):

1. How can we create scoped prediction windows on demand outside or irrespective of `GameplayAbilities`? For example, how can a fire and forget projectile locally predict a damage `GameplayEffect` when it hits an enemy?

> The PredictionKey system is not really meant to do this. Fundamentally this systems works by a client initiating a predictive action, telling the server about it with a key, and then both client and server running the same thing and associating predictive side effects with the given prediction key. For example, “I am predictively activating an ability” or “I have produced target data and am going to predictively run the part of the ability graph after the WaitTargetData task”.
>
> With this pattern, the PredictionKey “bounces” off the server and comes back to the client via UAbilitySystemComponent::ReplicatedPredictionKeyMap (replicated property). Once the key is replicated back from the server, the client is able to undo all of the locally predictive side effects (GameplayCues, GameplayEffects): the replicated versions *will be there* and if they aren’t then it was a misprediction. Knowing exactly when to undo the predictive side effects is crucial here: if you are too early you will see gaps, if you are too late you will have “double”. (Note this is referring to stateful prediction, like a looping GameplayCue of a duration based Gameplay Effect. “Burst” GameplayCues and instant Gameplay Effects are never “undone” or rolled back. They are just skipped on the client if there is a prediction key associated with them).
>
> To further hit home the point: it’s crucial that predictive action is something the server does not do on their own, but only does so when the client tells them to. So having a generic “Create a key on demand and tell the server so I can run something” does not work unless that “something” is something the server will only do once told to by the client.
>
> Backing up to the original question: something like a fire and forget projectile. Both Paragon and Fornite have projectile actor classes that use GameplayCues. However we do not use the Prediction Key system to do these. Instead we have a concept on Non-Replicated GameplayCues. GameplayCues that just fire off locally and are skipped by the server completely. Essentially all these are direct calls to UGameplayCueManager::HandleGameplayCue. They do not route through the UAbilitySystemComponent so no prediction key checks / early returns are made.
>
> The downside with non replicated GameplayCues is that, well, they are not replicated. So its up to the projectile class/blueprint to make sure the code paths that call these functions are running on everyone. We have for cues startup (called in BeginPlay), explosion, hit wall/character, etc.
>
> These type of events are already generated client side, so calling into a non replicated gameplay cue was no big deal. Complicated blueprints can be tricky, and are up to the author to make sure they understand what is running where.

2. When using a `WaitNetSync` `AbilityTask` with `OnlyServerWait` to create a scoped prediction window in a locally predicted `GameplayAbility`, could players potentially cheat by delaying their packets to the Server to control `GameplayAbility` timing since the Server is waiting for their RPC withtheir prediction key? Was this ever an issue in Paragon or Fortnite, and if so, what did Epic do to remedy it?

> Yes, this is a valid concern. Any ability blueprint running on the server that is waiting for a client “signal” is potentially vulnerable to lag switch type exploits.
>
> Paragon had a custom targeting task similar to UAbilityTask_WaitTargetData. In this task we had timeouts, or a “max delay” that we would wait on the client for instantaneous targeting modes. If the targeting mode was waiting for user confirmation (button press) then it would be ignored since the user is allowed to take his time. But for abilities that instantly confirmed targeting we would only wait a certain amount of time before either A) generating the target data server side or B) canceling the ability.
>
> We never had such mechanisms for WaitNetSync, which we used pretty sparingly.
>
> I don’t believe Fortnite makes use of anything like this though. The weapon abilities in Fortnite are special cased batched to a single fortnite-specific RPC: one RPC to activate the ability, provide target data, and end the ability. So weapon abilities are intrinsically not vulnerable to this in Battle Royale.
>
> My take is that this is something that could probably be solved system wide but I don’t see us making the change ourselves anytime soon. Spot fixing WaitNetSync to include a max delay for the case you mention is probably a reasonable task, but again - unlikely we will do this on our end in the immediate future.


3. Which `EGameplayEffectReplicationMode` did Paragon and Fortnite use and what are Epic’s recommendations for when to use each?

> Both games essentially use Mixed mode for their player controlled characters and Minimal for AI controlled (AI minions, jungle creeps, AI Husks, etc). This is what I would recommend most people using the system in a multiplayer game. The sooner into your project you set these, the better.
>
> Fortnite goes a few steps further with its optimizations. It actually does not replicate the UAbilitySystemComponent at all for simulated proxies. The component and attribute subobjects are skipped inside ::ReplicateSubobjects() on the owning fortnite player state class. We do push the bare minimum replicated data from the ability system component to a structure on the pawn itself (basically, a subset of attribute values and a white list subset of tags that we replicate down in a bitmask). We call this a “proxy”. On the receiving side we take the proxy data, replicated on the pawn, and push it back into ability system component on the player state. So you do have an ASC for each player in FNBR, it just doesn’t directly replicate: instead it replicates data via a minimal proxy struct on the pawn and then routes back to the ASC on receiving side. This is advantage since its A) a more minimal set of data B) takes advantage of pawn relevancy.
>
> I’m not sure if it is still necessary with other server side optimizations that have been done since then (Replication Graph, etc) and it is not the most maintainable pattern.


4. Since we cannot predict the removal of `GameplayEffects` as per `GameplayPrediction.h`, are there any strategies for mitigating the effects of latency on removing `GameplayEffects`? For example, when removing a movement speed slow, we currently have to wait for the Server to replicate the `GameplayEffect` removal resulting in a snap of the player’s character position.

> This is a tough one and I don’t have a good answer. We generally skirted around these problems with tolerances and smoothing. I totally agree that ability system and precise synchronization with the character movement system is not in a good place and something we do want to fix.
>
> I had a shelf of allowing predictive removal of GEs but could never work out all edge cases before having to move on. This doesn’t solve everything though since character movement still has an internal saved move buffer that does not know anything about the ability system and possible movement speed modifiers, etc. It is still possible to get into correction feedback loops even outside of not being able to predict the removal of GEs.
>
> If you think you have a case that is truly desperate, you are able to predictively add a GE that would inhibit your movement speed GEs. I’ve never done this myself but have theorized about it before. It may be able to help with a certain class of problem.


5. We know that the `AbilitySystemComponent` lives on the `PlayerState` in Paragon and Fortnite and on the `Character` in the Action RPG Sample. What are Epic’s internal rules, guidelines, or recommendations for where the AbilitySystemComponent should live, and what should its `Owner` be?

> In general I would say anything that does not need to respawn should have the Owner and Avatar actor be the same thing. Anything like AI enemies, buildings, world props, etc.
>
> Anything that does respawn should have the Owner and Avatar be different so that the Ability System Component does not need to be saved off / recreated / restored after a respawn. PlayerState is the logical choice it is replicated to all clients (where as PlayerController is not). The downside is PlayerStates are always relevant so you can run into problems in 100 player games (See notes on what FN did in question #3).


6. Is it viable to have several `AbilitySystemComponents` which have the same owner but different avatars (e.g. on pawn and weapon/items/projectiles with `Owner` set to `PlayerState`)?

> The first problem I see there would be implementing the IGameplayTagAssetInterface and IAbilitySystemInterface on the owning actor. The former may be possible: just aggregate the tags from all ASCs (but watch out - HasAllMatchingGameplayTags may be met only via cross ASC aggregation. It wouldn't be enough to just forward that calls to each ASC and OR the results together). But the later is even trickier: which ASC is the authoritative one? If someone wants to apply a GE - which one should receive it? Maybe you can work these out but this side of the problem will be the hardest: owners will multiple ASCs beneath them.
>
> Separate ASCs on the pawn and the weapon can make sense on its own though. E.g, distinguishing between tags the describe the weapon vs those that describe the owning pawn. Maybe it does make sense that tags granted to the weapon also “apply” to the owner and nothing else (E.g, attributes and GEs are independent but the owner will aggregate the owned tags like I describe above). This could work out, I am sure. But having multiple ASCs with the same owner may get dicey.


7. Is there a way to stop the Server from overwriting the cooldown duration of locally predicted abilities on the Owning Client? In scenarios of high latency, this would let the Owning Client "try" to activate the ability again when its local cooldown expires but it is still on cooldown on the Server. By the time the Owning Client's activation request reaches the Server over the network, the Server may be off cooldown or the Server might be able to queue the activation request for the remaining milliseconds that it has left. Otherwise as is, clients with higher latency have a longer delay before when they can reactivate an ability versus those with less latency. This is most apparent with very low cooldown abilities like a basic attack that can be less than one second of cooldown. If there isn't a way to stop the Server from overwriting the cooldown duration of locally predicted abilities, what is Epic's strategy for mitigating the effects of high latency on reactivating abilities? To word it another example-based way, how did Epic design Paragon's basic attacks and other abilities so that high latency players could attack or activate at the same speed as low latency players with local prediction?

> The short answer there is not a way to prevent this and Paragon definitely had the problem. Higher latency connections would have a lower ROF with basic attacks.
>
> I attempted to fix this by adding “GE reconciliation” where latency was taken into account when calculating GE duration. Essentially allowing the server to eat some of the total GE time so that the effective time of the GE client side would be 100% consistent with any amount of latency (though fluctuations could still cause issues). However I never got this working in a state that could ship and the project moved fast and we just never fully addressed it.
>
> Fortnite does its own bookkeeping for weapon firing rates: it does not use GEs for cooldowns on weapons. I would recommend this if this is a critical problem for your game.


8. What is Epic’s roadmap for the GameplayAbilitySystem plugin? Which features does Epic plan to add in 2019 and beyond?

> We feel that overall the system is pretty stable at this point and we don’t have anyone working on major new features. Bug fixes and small improvements occasionally are made for Fortnite or from UDN/pull requests, but that is it right now.
>
> Longer term, I think we will eventually do a “V2” or some big changes. We learned a lot from writing this system and feel we got a lot right and a lot wrong. I would love a chance to correct those mistakes and improve some of the fatal flaws that were pointed out above.
>
> If a V2 was to ever come, providing an upgrade path would be of utmost importance. We would never make a V2 and leave Fortnite on V1 forever: there would be some path or procedures that would automatically migrate as much as possible, though there would still almost certainly be some manual remaking required.
>
> The high priority fixes would be:
> * Better interoperability with the character movement system. Unifying client prediction.
> * GE removal prediction (question #4)
> * GE latency reconciliation (question #7)
> * Generalized network optimizations such as batching RPCs and proxy structures. Mostly the stuff that we’ve done for Fortnite but find ways to break it down into more generalized form, at least so that games can write their own game specific optimizations more easily.
>
> The more general refactor type of changes I would consider making:
> * I would like to look at fundamentally moving away from having GEs reference spreadsheet values directly, instead they would be able to emit parameters and those parameters could be filled by some higher level object that is bound to spreadsheet values. The problem with the current model is that GEs become unsharable due to their tight coupling with the curve table rows. I think a generalized system for parameterization could be written and be the underpinning of a V2 system.
> * Reduce number of “policies” on UGameplayAbility. I would remove ReplicationPolicy and InstancingPolicy. Replication is, imo, almost never actually needed and causes confusion. InstancingPolicy should be replaced instead by making FGameplayAbilitySpec a UObject that can be subclassed. This should have been the “non instantiated ability object” that has events and is blueprintable. The UGameplayAbility should be the “instanced per execution” object. It could be optional if you need to actually instantiate: instead “non instanced” abilities would be implemented via the new UGameplayAbilitySpec object. 
> * The system should provide more “middle level” constructs such as “filtered GE application container” (data drive what GEs to apply to which actors with higher level gameplay logic), “Overlapping volume support” (apply the “Filtered GE application container” based on collision primitive overlap events), etc. These are building blocks that every project ends up implementing in their own way. Getting them right is non trivial so I think we should do a better job providing some basic implementations. 
> * In general, reducing boilerplate needed to get your project up and running. Possibly a separate module “Ex library” or whatever that could provide things like passive abilities or basic hitscan weapons out of the box. This module would be optional but would get you up and running quickly.
> * I would like to move GameplayCues to a separate module that is not coupled with the ability system. I think there are a lot of improvements that could be made here.


> This is only my personal opinion and not a commitment from anyone. I think the most realistic course of action will be as new engine tech initiatives come through, the ability system will need to be updated and that will be a time to do this sort of thing. These initiatives could be related to scripting, networking, or physics/character movement. This is all very far looking ahead though so I cannot give commitments or estimates on timelines.

**[⬆ Back to Top](#table-of-contents)**

<a name="resources-daveratti-community2"></a>
#### 11.1.2 Community Questions 2
Community member [iniside](https://github.com/iniside)'s Q&A with Dave Ratti:

1. Is the support for decoupled fixed ticking planned? I'd like to
have Game Thread be fixed (like 30/60fps) and let the rendering thread
run wild. I ask if this is something we should expect in future or
not, to make some assumptions about how gameplay should work.
I ask mainly because there is now a fixed async tick for physics and
this poses a question how the rest of the system might work in the
future. I do not hide that having the ability to have fixed tick game
thread without also fixing tick rate of the rest of the engine would
be beyond awesome.

> There are no plans to decouple rendering frame rate and game thread tick frame rate. I think the ship has sailed on this ever happening due to the complexity of these systems and the requirement to preserve backwards compatibility with previous engine versions.
>
> Instead, the direction we've gone is to have an asynchronous "Physics Thread" which runs at a fixed tick rate, independent of the game thread. Things that need to run at a fixed rate can run here and the game thread / rendering can operate how they always have.
>
> It's worth clarifying that Network Prediction supports what it calls Independent Ticking and Fixed Ticking modes. My long term plan is to keep Independent Ticking roughly how it is today in Network Prediction where it runs on the game thread at variable frame rate and there is no "group/world" prediction, it's just the classic "clients predict their own pawn and owned actors" model. And Fixed Ticking would be what uses the async physics stuff and allows you to predict non client controlled/owned actors like physics objects and other clients/pawns/vehicles/etc.


2. Is there any plan on how the integration of Network Prediction will
look with the Ability System? Like for example, fixed frame ability
activation (so the server gets frames in which abilities were
activated and tasks executed instead of prediction keys)?

> Yes, the plan is to rewrite/remove the Ability System's prediction keys and replace them with Network Prediction constructs. The MockAbility examples in NetworkPredictionExtras show how this might work but they are more "hard coded" than what GAS will require. 
>
> The main idea would be that we remove the explicit client->server Prediction Key exchange in the ASC's RPCs. There would no longer be prediction windows or scoped prediction keys. Instead everything would be anchored around NetworkPrediction frames. The important thing is that client and server agree on when things happen. Examples would be:
>
> * When abilities were activated/ended/cancelled
> * When Gameplay Effects were applied/removed
> * Attribute values (what an attributes value was at frame X)
>
> I think this could be done generically at the ability system level. But actually making the user-defined logic inside a UGameplayAbility completely rollback-able would still take more work. We may end up having a subclass of UGameplayAbility that is fully rollbackable and has access to a more limited set of functionality or only Ability Tasks that are marked as rollback-friendly. Something like that. There are also many implications to animation events and root motion and how those are processed.
>
> Wish I had a more clear answer but it's really important we get the foundation right before touching GAS again. Movement and physics have to be solid before the higher level systems can be changed.


3. Is there a plan to move Network Prediction development toward the
main branch? Not gonna lie, I'd really like to check the latest code.
Regardless of it's state.

> We are working towards it. The system work is still all being done in NetworkPrediction (see NetworkPhysics.h) and the underlying async physics stuff should be all available (RewindData.h etc). But we also have use cases in Fortnite that we have been focused on that obviously can't be made public. We are working through bugs, performance optimizations, etc.
>
> For more context: when working on the early versions of this system, we were very focused on the "front end" of things - how state and simulations were defined and written. We learned a lot there. But as the async physics stuff has come online, we've been much more focused on just getting something real to work in this system, at the expense of throwing out some of our early abstractions. The goal here is to circle back when the real thing is working and reunifying things. E.g, get back to the "front end" and make the final version of that on top of the core pieces of tech we are working on now.


4. For some time on main branch there was a plugin for sending Gameplay
Messages (Looked like Event/Message Bus), but it was removed. Any
plans to restore it? With the Game Features/Modular Gameplay plugins,
having a generic Event Bus Dispatcher would be extremely useful.

> I think you are referring to the GameplayMessages plugin. This will probably come back at some point - the API isn't really finalized yet and the author didn't mean for it to be public yet. I agree it should be useful for modular gameplay design. But it's not really my area so I don't have much more information. 


5. I've been playing recently with async fixed physics and the results
are promising, though if there is going to be NP update in the future
I will probably just play around and wait, since to get it working I
still need to get entire engine into fixed tick and on the other hand
I try to keep physics at 33ms. Which does not make for a good
experience if everything is at 30 fps (:.

I have noticed there was some work on Async
CharacterMovementComponent, but not sure if this will be using Network
Prediction, or it is a separate effort?

Since I noticed it, I also went ahead and tried to implement my custom
async movement at fixed tick rate, which worked okay, but on top of it
I also needed to add a separate update for interpolation. The setup
was to run simulation tick on separate worker threads at fixed 33ms
update, do calculations, save result, and interpolate it at the game
thread to match current frame rate. Not perfect, but it got the job
done.

My question is, if this is something that might be easier to set up in
the future, as there is just quite a bit of boilerplate code to write,
(the interpolation part) and it's not particularly efficient to
interpolate each moving object individually.

The async stuff is really interesting, because it would allow you to
really run game simulation at fixed update rate (which would make
fixed thread unneeded) and have more predictable results. Is this
something that is intended going forward, or more of a benefit to
select systems? As far as I remember actor transforms are not updated
async and blueprints are not entirely thread safe. In other words is
it something that is planned to be supported at more of a framework
level or something that each game has to solve on it's own?

> Async CharacterMovementComponent
>
> This is basically an early prototype/experiment of porting CMC as it is to the physics thread. I don't view it as the future of CMC yet, but it could evolve into that. Right now there is no networking support so it's not something I would really follow. The people doing it are mostly concerned with measuring input latency that this system would add and how that could be mitigated.
>
> I still need to get entire engine into fixed tick and on the other hand I try to keep physics at 33ms. Which does not make for a good experience if everything is at 30 fps (:.
>
> The async stuff is really interesting, because it would allow you to really run game simulation at fixed update rate (which would make fixed thread unneeded) 
>
> Yes. The goal here is that with async physics enabled, you can run the engine at variable tick rate while the physics and "core" gameplay simulations can run at the fixed rate (such as character movement, vehicles, GAS, etc).
>
> These are the cvars that need to be set to enable this now: (I think you've figured this out)  
> `p.DefaultAsyncDt=0.03333`  
> `p.RewindCaptureNumFrames=64`
>
> Chaos does provide interpolation for the physics state (e.g, the transforms that get pushed back to the UPrimitiveComponent and are visible to the game code). There is a cvar now, `p.AsyncInterpolationMultiplier`, which controls that if you want to look at it. You should see smooth continuous motion of physics bodies without having to write any extra code. 
>
> If you want to interpolate non physics state, it is still up to you to do that right now. The example would be like a cool-down that you want to update (tick) on the async physics thread but see smooth continuous interpolation on the game thread so that every render frame the cool-down visualization is updated. We will get to this eventually but don't have examples yet.
>
> there is just quite a bit of boilerplate code to write,
>
> Yeah, so that has been a big general problem with the system up until now. We want to provide an interface that experienced programmers can use to maximize performance and safety (the ability to write gameplay code that "just works" predictively without tons of hazards and things you could-do-but-better-not). So something like CharacterMoverment might do a bunch of custom stuff to maximize its performance - e.g, writing templated code and doing batch updating, going wide, breaking the update loop into distinct phases etc. We want to provide a good "low level" interface into the async thread and rollback systems for this use case. And in this case too - it's still reasonable that the character movement system itself is extendable in its own way. For example providing a way to blueprint a custom movement mode and providing a blueprint API that is thread safe.
>
> But we recognize this is not acceptable for simpler gameplay objects that don't really need their own "system". Something more inline with Unreal is what is needed. E.g, using the reflection system, having general blueprint support, etc. There are examples of blueprints being used on other threads (see BlueprintThreadSafe keyword and what the animation system has been working towards). So I think there will be some form of this one day. But again, we aren't there yet.
>
> I realize you were just asking about interpolation but that is the general answer: right now we have you do everything manually like NetSerialize, ShouldReconcile, Interpolate, etc but eventually we'll have a way that is like "if you want to just use the reflection system, you don't have to manually write this stuff". We just don't want to *force* everyone to use the reflection system since that imposes other limitations that we think we don't want to take on the lowest levels of the system. 
>
> And then just to tie this back to what I said earlier - right now we are really focused on getting a few very specific examples working and performant and then we will turn attention back to the front end and making things friendly to use and iterate on, reducing boilerplate, etc for everybody else to use. 

**[⬆ Back to Top](#table-of-contents)**

<a name="changelog"></a>
## 12. GAS Changelog

This is a list of notable changes (fixes, changes, and new features) to GAS compiled from the official Unreal Engine upgrade changelog and from undocumented changes that I've encountered. If you've found something that isn't listed here, please make an issue or pull request.

<a name="changelog-5.3"></a>
### 5.3
* Crash Fix: Fixed a crash when trying to apply Gameplay Cues after a seamless travel.
* Crash Fix: Fixed a crash caused by GlobalAbilityTaskCount when using Live Coding.
* Crash Fix: Fixed UAbilityTask::OnDestroy to not crash if called recursively for cases like UAbilityTask_StartAbilityState.
* Bug Fix: It is now safe to call `Super::ActivateAbility` in a child class. Previously, it would call `CommitAbility`.
* Bug Fix: Added support for properly replicating different types of FGameplayEffectContext.
* Bug Fix: FGameplayEffectContextHandle will now check if data is valid before retrieving "Actors".
* Bug Fix: Retain rotation for Gameplay Ability System Target Data LocationInfo.
* Bug Fix: Gameplay Ability System now stops searching for PC only if a valid PC is found.
* Bug Fix: Use existing GameplayCueParameters if it exists instead of default parameters object in RemoveGameplayCue_Internal.
* Bug Fix: GameplayAbilityWorldReticle now faces towards the source Actor instead of the TargetingActor.
* Bug Fix: Cache trigger event data if it was passed in with GiveAbilityAndActivateOnce and the ability list was locked.
* Bug Fix: Support has been added for the FInheritedGameplayTags to update its CombinedTags immediately rather than waiting until a Save.
* Bug Fix: Moved ShouldAbilityRespondToEvent from client-only code path to both server and client.
* Bug Fix: Fixed FAttributeSetInitterDiscreteLevels from not working in Cooked Builds due to Curve Simplification.
* Bug Fix: Set CurrentEventData in GameplayAbility.
* Bug Fix: Ensure MinimalReplicationTags are set up correctly before potentially executing callbacks.
* Bug Fix: Fixed ShouldAbilityRespondToEvent from not getting called on the instanced GameplayAbility.
* Bug Fix: Gameplay Cue Notify Actors executing on Child Actors no longer leak memory when gc.PendingKill is disabled.
* Bug Fix: Fixed an issue in GameplayCueManager where GameplayCueNotify_Actors could be 'lost' due to hash collisions.
* Bug Fix: WaitGameplayTagQuery will now respect its Query even if we have no Gameplay Tags on the Actor.
* Bug Fix: PostAttributeChange and AttributeValueChangeDelegates will now have the correct OldValue.
* Bug Fix: Fixed FGameplayTagQuery from not showing a proper Query Description if the struct was created by native code.
* Bug Fix: Ensure that the UAbilitySystemGlobals::InitGlobalData is called if the Ability System is in use. Previously if the user did not call it, the Gameplay Ability System did not function correctly.
* Bug Fix: Fixed issue when linking/unlinking anim layers from UGameplayAbility::EndAbility.
* Bug Fix: Updated Ability System Component function to check the Spec's ability pointer before use.
* New: Added a GameplayTagQuery field to FGameplayTagRequirements to enable more complex requirements to be specified.
* New: Introduced FGameplayEffectQuery::SourceAggregateTagQuery to augment SourceTagQuery.
* New: Extended the functonality to execute and cancel Gameplay Abilities & Gameplay Effects from a console command.
* New: Added the ability to perform an "Audit" on Gameplay Ability Blueprints that will show information on how they're developed and intended to be used.
* Change: OnAvatarSet is now called on the primary instance instead of the CDO for instanced per Actor Gameplay Abilities.
* Change: Allow both Activate Ability and Activate Ability From Event in the same Gameplay Ability Graph.
* Change: AnimTask_PlayMontageAndWait now has a toggle to allow Completed and Interrupted after a BlendOut event.
* Change: ModMagnitudeCalc wrapper functions have been declared const.
* Change: FGameplayTagQuery::Matches now returns false for empty queries.
* Change: Updated FGameplayAttribute::PostSerialize to mark the contained attribute as a searchable name.
* Change: Updated GetAbilitySystemComponent to default parameter to Self.
* Change: Marked functions as virtual in AbilityTask_WaitTargetData.
* Change: Removed unused function FGameplayAbilityTargetData::AddTargetDataToGameplayCueParameters.
* Change: Removed vestigial GameplayAbility::SetMovementSyncPoint.
* Change: Removed unused replication flag from Gameplay tasks & Ability system components.
* Change: Moved some gameplay effect functionality into optional components. All existing content will automatically update to use components during PostCDOCompiled, if necessary.

https://docs.unrealengine.com/5.3/en-US/unreal-engine-5.3-release-notes/

<a name="changelog-5.2"></a>
### 5.2
* Bug Fix: Fixed a crash in the `UAbilitySystemBlueprintLibrary::MakeSpecHandle` function.
* Bug Fix: Fixed logic in the Gameplay Ability System where a non-Controlled Pawn would be considered remote, even if it was spawned locally on the server (e.g. Vehicles).
* Bug Fix: Correctly set activation info on predicted instanced abilities that were rejected by the server.
* Bug Fix: Fixed a bug that would cause GameplayCues to get stuck on remote instances.
* Bug Fix: Fixed a memory stomp when chaining calls to WaitGameplayEvent.
* Bug Fix: Calling the AbilitySystemComponent `GetOwnedGameplayTags()` function in Blueprint no longer retains the previous call's return values when the same node is executed multiple times.
* Bug Fix: Fixed an issue with GameplayEffectContext replicating a reference to a dynamic object that would never be replicated.
  * This prevented GameplayEffect from calling `Owner->HandleDeferredGameplayCues(this)` as `bHasMoreUnmappedReferences` would always be true.
* New: The [Gameplay Targeting System](https://docs.unrealengine.com/en-US/gameplay-targeting-system-in-unreal-engine/) is a way to create data-driven targeting requests.
* New: Added custom serialization support for GameplayTag Queries.
* New: Added support for replicating derived FGameplayEffectContext types.
* New: Gameplay Attributes in assets are now registered as searchable names on save, allowing for references to attributes to be seen in the reference viewer.
* New: Added some basic unit tests for the AbilitySystemComponent.
* New: Gameplay Ability System Attributes now respect Core Redirects. This means you can now rename Attribute Sets and their Attributes in code and have them load properly in assets saved with the old names by adding redirect entries to DefaultEngine.ini.
* Change: Allow changing the evaluation channel of a Gameplay Effect Modifier from code.
* Change: Removed previously unused variable `FGameplayModifierInfo::Magnitude` from the Gameplay Abilities Plugin.
* Change: Removed the synchronization logic between the ability system component and Smart Object instance tags.

https://docs.unrealengine.com/5.2/en-US/unreal-engine-5.2-release-notes/

<a name="changelog-5.1"></a>
### 5.1
* Bug Fix: Fixed issue where replicated loose gameplay tags were not replicating to the owner.
* Bug Fix: Fixed AbilityTask bug where abilities could be blocked from timely garbage-collection.
* Bug Fix: Fixed an issue when a gameplay ability listening to activate based on a tag would fail to be activated. This would happen if there were more than one Gameplay Ability listening to this tag, and the first one in the list was invalid or didn't have authority to activate.
* Bug Fix: Fixed GameplayEffects that use Data Registries correctly from warning on load and improved the warning text.
* Bug Fix: Removed code from UGameplayAbility that was incorrectly only registering the last instanced ability with the Blueprint debugger for breakpoints.
* Bug Fix: Fixed Gameplay Ability System Ability getting stuck if EndAbility was called during the lock inside ApplyGameplayEffectSpecToTarget.
* New: Added support for Gameplay Effects to add blocked ability tags.
* New: Added WaitGameplayTagQuery nodes. One is based off of the UAbilityTask and the other is of UAbilityAsync. This node specifies a TagQuery, and will trigger its output pin when the query becomes true or false, based on configuration.
* New: Modified AbilityTask debugging in Console Variables to enable debug recording and printing to log by default in non-shipping builds (with ability to hotfix on/off as needed).
* New: You can now set AbilitySystem.AbilityTask.Debug.RecordingEnabled to 0 to disable, 1 to enable in non-shipping builds, and 2 to enable all builds (including shipping).
* New: You can use AbilitySystem.AbilityTask.Debug.AbilityTaskDebugPrintTopNResults to only print the top N results in log (to avoid log spam).
* New: STAT_AbilityTaskDebugRecording can be used to test perf impact from these on-by-default debugging changes.
* New: Added a debug command to filter GameplayCue events.
* New: Added new debug commandsAbilitySystem.DebugAbilityTags, AbilitySystem.DebugBlockedTags, andAbilitySystem.DebugAttribute to the Gameplay Ability System.
* New: Added a Blueprint function to get a debug string representation of a Gameplay Attribute.
* New: Added a new Gameplay Task resource overlap policy to cancel existing tasks.
* Change: Now Ability Tasks should make sure to call Super::OnDestroy only after they do anything needed to the Ability pointer, as it will be nulled out after calling it.
* Change: Converted FGameplayAbilitySpec/Def::SourceObject to be a weak reference.
* Change: Made a Ability System Component reference in the Ability Task a weak pointer so Garbage Collection can delete it.
* Change: Removed redundant enum EWaitGameplayTagQueryAsyncTriggerCondition.
* Change: GameplayTasksComponent and AbilitySystemComponent now support the registered subobject API.
* Change: Added better logging to indicate why Gameplay Abilities failed to be activated.
* Change: Removed AbilitySystem.Debug.NextTarget and PrevTarget commands in favor of global HUD NextDebugTarget and PrevDebugTarget commands.

https://docs.unrealengine.com/5.1/en-US/unreal-engine-5.1-release-notes/

<a name="changelog-5.0"></a>
### 5.0

https://docs.unrealengine.com/5.0/en-US/unreal-engine-5.0-release-notes/

<a name="changelog-4.27"></a>
### 4.27
* Crash Fix: Fixed a root motion source issue where a networked client could crash when an Actor finishes executing an ability that uses a constant force root motion task with a strength-over-time modifier.
* Bug Fix: Fixed a regression in Editor loading time when using GameplayCues.
* Bug Fix: GameplayEffectsContainer's `SetActiveGameplayEffectLevel` method will no longer dirty FastArray if setting the same EffectLevel.
* Bug Fix: Fixed an edge case in GameplayEffect mixed replication mode where Actors not explicitly owned by the net connection but who utilize that connection from `GetNetConnection` will not received mixed replication updates.
* Bug Fix: Fixed an endless recursion occuring in GameplayAbility's class method `EndAbility` which was called by calling `EndAbility` again from `K2_OnEndAbility`.
* Bug Fix: GameplayTags Blueprint pins will no longer be silently cleared if they are loaded before tags are registered. They now work the same as GameplayTag variables, and the behavior for both can be changed with the ClearInvalidTags option in the Project Settings.
* Bug Fix: Improved thread safety of GameplayTag operations.
* New: Exposed SourceObject to GameplayAbility's `K2_CanActivateAbility` method.
* New: Native GameplayTags. Introducing a new `FNativeGameplayTag`, these make it possible to do one off native tags that are correctly registered and unregistered when the module is loaded and unloaded.
* New: Updated `GiveAbilityAndActivateOnce` to pass in FGameplayEventData parameter.
* New: Improved ScalableFloats in the GameplayAbilities plugin to support dynamic lookup of curve tables from the new Data Registry System. Added a ScalableFloat header for easier reuse of the generic struct outside the abilities plugin.
* New: Added code support for using the GameplayTag UI in other Editor customizations via GameplayTagsEditorModule.
* New: Modified UGameplayAbility's PreActivate method to optionally take in trigger event data.
* New: Added more support to filter GameplayTags in the Editor using a project-specific filter. `OnFilterGameplayTag` supplies the referencing property and the tag source, so you can filter tags based on what asset is requesting the tag.
* New: Added option to preserve the original captured SourceTags when GameplayEffectSpec's class method `SetContext` is called after initialization.
* New: Improved UI for registering GameplayTags from specific plugins. The new tag UI now lets you select a plugin location on disk for newly added GameplayTag sources.
* New: A new track has been added to Sequencer to allow for triggering notify states on Actors built using the GameplayAbiltiySystem. Like notifies, the GameplayCueTrack can utilize range-based events or trigger-based events.
* Change: Changed the GameplayCueInterface to pass GameplayCueParameters struct by reference.
* Optimization: Made several performance improvements to loading and regenerating the GameplayTag table were implemented so that this option would be optimized.

https://docs.unrealengine.com/en-US/WhatsNew/Builds/ReleaseNotes/4_27/

<a name="changelog-4.26"></a>
### 4.26
* GAS plugin is no longer flagged as beta.
* Crash Fix: Fixed a crash when adding a gameplay tag without a valid tag source selection.
* Crash Fix: Added the path string arg to a message to fix a crash in UGameplayCueManager::VerifyNotifyAssetIsInValidPath.
* Crash Fix: Fixed an access violation crash in AbilitySystemComponent_Abilities when using a ptr without checking it.
* Bug Fix: Fixed a bug where stacking GEs that did not reset the duration on additional instances of the effect being applied.
* Bug Fix: Fixed an issue that caused CancelAllAbilities to only cancel non-instanced abilities.
* New: Added optional tag parameters to gameplay ability commit functions.
* New: Added StartTimeSeconds to PlayMontageAndWait ability task and improved comments.
* New: Added tag container "DynamicAbilityTags" to FGameplayAbilitySpec. These are optional ability tags that are replicated with the spec. They are also captured as source tags by applied gameplay effects.
* New: GameplayAbility IsLocallyControlled and HasAuthority functions are now callable from Blueprint.
* New: Visual logger will now only collect and store info about instant GEs if we're currently recording visual logging data.
* New: Added support for redirectors on gameplay attribute pins in blueprint nodes.
* New: Added new functionality for when root motion movement related ability tasks end they will return the movement component's movement mode to the movement mode it was in before the task started.

https://docs.unrealengine.com/en-US/WhatsNew/Builds/ReleaseNotes/4_26/

<a name="changelog-4.25.1"></a>
### 4.25.1
* Fixed! UE-92787 Crash saving blueprint with a Get Float Attribute node and the attribute pin is set inline
* Fixed! UE-92810 Crash spawning actor with instance editable gameplay tag property that was changed inline

<a name="changelog-4.25"></a>
### 4.25
* Fixed prediction of `RootMotionSource` `AbilityTasks`
* [`GAMEPLAYATTRIBUTE_REPNOTIFY()`](#concepts-as-attributes) now additionally takes in the old `Attribute` value. We must supply that as the optional parameter to our `OnRep` functions. Previously, it was reading the attribute value to try to get the old value. However, if called from a replication function, the old value had already been discarded before reaching SetBaseAttributeValueFromReplication so we'd get the new value instead.
* Added [`NetSecurityPolicy`](#concepts-ga-netsecuritypolicy) to `UGameplayAbility`.
* Crash Fix: Fixed a crash when adding a gameplay tag without a valid tag source selection.
* Crash Fix: Removed a few ways for attackers to crash a server through the ability system.
* Crash Fix: We now make sure we have a GameplayEffect definition before checking tag requirements.
* Bug Fix: Fixed an issue with gameplay tag categories not applying to function parameters in Blueprints if they were part of a function terminator node.
* Bug Fix: Fixed an issue with gameplay effects' tags not being replicated with multiple viewports.
* Bug Fix: Fixed a bug where a gameplay ability spec could be invalidated by the InternalTryActivateAbility function while looping through triggered abilities.
* Bug Fix: Changed how we handle updating gameplay tags inside of tag count containers. When deferring the update of parent tags while removing gameplay tags, we will now call the change-related delegates after the parent tags have updated. This ensures that the tag table is in a consistent state when the delegates broadcast.
* Bug Fix: We now make a copy of the spawned target actor array before iterating over it inside when confirming targets because some callbacks may modify the array.
* Bug Fix: Fixed a bug where stacking GameplayEffects that did not reset the duration on additional instances of the effect being applied and with set by caller durations would only have the duration correctly set for the first instance on the stack. All other GE specs in the stack would have a duration of 1 second. Added automation tests to detect this case.
* Bug Fix: Fixed a bug that could occur if handling gameplay event delegates modified the list of gameplay event delegates.
* Bug Fix: Fixed a bug causing GiveAbilityAndActivateOnce to behave inconsistently.
* Bug Fix: Reordered some operations inside FGameplayEffectSpec::Initialize to deal with a potential ordering dependency.
* New: UGameplayAbility now has an OnRemoveAbility function. It follows the same pattern as OnGiveAbility and is only called on the primary instance of the ability or the class default object.
* New: When displaying blocked ability tags, the debug text now includes the total number of blocked tags.
* New: Renamed UAbilitySystemComponent::InternalServerTryActiveAbility to UAbilitySystemComponent::InternalServerTryActivateAbility.Code that was calling InternalServerTryActiveAbility should now call InternalServerTryActivateAbility.
* New: Continue to use the filter text for displaying gameplay tags when a tag is added or deleted. The previous behavior cleared the filter.
* New: Don't reset the tag source when we add a new tag in the editor.
* New: Added the ability to query an ability system component for all active gameplay effects that have a specified set of tags. The new function is called GetActiveEffectsWithAllTags and can be accessed through code or blueprints.
* New: When root motion movement related ability tasks end they now return the movement component's movement mode to the movement mode it was in before the task started.
* New: Made SpawnedAttributes transient so it won't save data that can become stale and incorrect. Added null checks to prevent any currently saved stale data from propagating. This prevents problems related to bad data getting stored in SpawnedAttributes.
* API Change: AddDefaultSubobjectSet has been deprecated. AddAttributeSetSubobject should be used instead.
* New: Gameplay Abilities can now specify the Anim Instance on which to play a montage.

https://docs.unrealengine.com/en-US/WhatsNew/Builds/ReleaseNotes/4_25/

<a name="changelog-4.24"></a>
### 4.24
* Fixed blueprint node `Attribute` variables resetting to `None` on compile.
* Need to call [`UAbilitySystemGlobals::InitGlobalData()`](#concepts-asg-initglobaldata) to use [`TargetData`](#concepts-targeting-data) otherwise you will get `ScriptStructCache` errors and clients will be disconnected from the server. My advice is to always call this in every project now whereas before 4.24 it was optional.
* Fixed crash when copying a `GameplayTag` setter to a blueprint that didn't have the variable previously defined.
* `UGameplayAbility::MontageStop()` function now properly uses the `OverrideBlendOutTime` parameter.
* Fixed `GameplayTag` query variables on components not being modified when edited.
* Added the ability for `GameplayEffectExecutionCalculations` to support scoped modifiers against "temporary variables" that aren't required to be backed by an attribute capture.
  * Implementation basically enables `GameplayTag`-identified aggregators to be created as a means for an execution to expose a temporary value to be manipulated with scoped modifiers; you can now build formulas that want manipulatable values that don't need to be captured from a source or target.
  * To use, an execution has to add a tag to the new member variable `ValidTransientAggregatorIdentifiers`; those tags will show up in the calculation modifier array of scoped mods at the bottom, marked as temporary variables—with updated details customizations accordingly to support feature
* Added restricted tag quality-of-life improvements. Removed the default option for restricted `GameplayTag` source. We no longer reset the source when adding restricted tags to make it easier to add several in a row. 
* `APawn::PossessedBy()` now sets the owner of the `Pawn` to the new `Controller`. Useful because [Mixed Replication Mode](#concepts-asc-rm) expects the owner of the `Pawn` to be the `Controller` if the `ASC` lives on the `Pawn`.
* Fixed bug with POD (Plain Old Data) in `FAttributeSetInitterDiscreteLevels`.

https://docs.unrealengine.com/en-US/WhatsNew/Builds/ReleaseNotes/4_24/

**[⬆ Back to Top](#table-of-contents)**
