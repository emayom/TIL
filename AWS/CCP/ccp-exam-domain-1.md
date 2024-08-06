# AWS Certified Cloud Practitioner(CLF-C02) 
### Domain 1: Cloud Concepts
#### 1.1 AWS 클라우드의 이점 정의
- **클라우드 컴퓨팅이란?**  
    - <u>on-demand delivery</u>  
        컴퓨팅 파워, 데이터베이스 저장소, 애플리케이션, 다른 IT 리소스들을 온디맨드로 제공하는 것
    - <u>pay-as-you-go pricing</u>  
        클라우드 서비스 플랫폼을 통해 종량 과금제 방식 제공
    - provision exactly the right type and size of computing   
      필요한 컴퓨팅 리소스의 유형과 크기에 따라 프로비저닝 가능
    - almost instantly 
    - simple way to access    

- **클라우드 컴퓨팅의 6가지 이점** 
    - 고정 비용을 가변 비용으로 전환 
        - 초기 자본 지출(CAPEX) 절감 
        - 총 소유 비용(TCO)와 운영 비용(OPEX) 절감
    - 규모의 경제(economies of scale)의 이점
      - 높은 규모의 경제 달성을 통한 단위당 비용 감소 
    - 용량 추정 불필요 
    - 조직 민첩성 증가
    - 데이터 센터 운영 및 유지 비용 불필요
    - 글로벌 배포 가능(글로벌 인프라)

- **클라우드 컴퓨팅 모델**
    | On-premises | IaaS | PaaS | SaaS | 
    | --- | :---: | :---: | :---: |
    | Applications | - | - | ✓ |
    | Data | - | - | ✓ | 
    | Runtime | - | ✓ | ✓ | 
    | OS | - | ✓ | ✓ | 
    | Virtualization | ✓ | ✓ | ✓ | 
    | Servers | ✓ | ✓ | ✓ | 
    | Storage | ✓ | ✓ | ✓ | 
    | Networking | ✓ | ✓ | ✓ | 

    - IaaS(Infrastructure as a Service)
        - 높은 유연성과 관리 제어 기능 제공 
        - Amazon EC2 (on AWS), GCP, Azure, Rackspace, Digital Ocean, Linode
    - PaaS(Platform as a Service)
        - 애플리케이션 배포 및 관리에 집중 
        - Elastic Beanstalk (on AWS), Heroku, Google App Engine(GCP), Window Azure(Microsoft)
    - Saas(Software as a Service)
        - 서비스 제공 업체가 완전히 운영 및 관리 
        - Many AWS services, Google Apps(Gmail), Dropbox, Zoom

- **클라우드 컴퓨팅 배포 모델**
    - Private Cloud(e.g. rackspace)
        - 온프레미스 방식(on-premises)
        - 단일 조직 (외부에 노출되지 않음)
        - 완전 제어
        - 강화된 보안 제공 

    - Public Cloud(e.g. MS Azure, Google Cloud, AWS)
        - 서드파티(third- party) 클라우드 서비스 제공 업체가 소유, 운영하는 클라우드 리소스를 인터넷을 통해 제공

    - Hybrid Cloud
        - (Private + Public) Cloud
        - 일부 서버 온프레미스로 유지, 일부 클라우드 확장 
        - 일부 민감 에셋 제어, 유연함 및 비용 효율성 만족 

#### 1.2 AWS 클라우드의 설계 원칙 파악
- **AWS Well-Architected Framework의 원칙** 
    - **운영 우수성(Operational excellence)**  
        - 소프트웨어를 올바르게 구축하는 동시에 우수한 고객 환경을 지속적으로 제공하기 위한 노력

        - 설계 원칙
            - 비즈니스 성과를 중심으로 팀 구성
            - 실행 가능한 인사이트를 위한 관찰성 구현
            - 자동화가 가능한 부분은 안전하게 자동화
            - 되돌릴 수 있도록 변경 사항을 조금씩 자주 적용
            - 운영 절차를 자주 개선
            - 장애 예측(Anticipate failure)
            - 모든 운영 이벤트와 장애로부터 배운 내용을 바탕으로 개선
            - 관리형 서비스 사용

    - **보안(Security)**  
        클라우드 기술을 활용하여 보안을 강화하고 데이터, 시스템 및 자산을 보호하는 능력

        - 설계 원칙
            - 강력한 자격 증명 기반 구현(최소 권한의 원칙)
            - 추적 기능 활성화
            - 모든 계층에 보안 적용
            - 보안 모범 사례 자동화 
            - 전송 및 저장 중인 데이터 보호
            - 데이터에 쉽게 액세스할 수 없도록 유지
            - 보안 이벤트에 대비

    - **안정성(Reliability)**  
        워크로드의 기능이 필요한 때에 기능을 정확하고 일관되게 수행하는 역량

        - 설계 원칙
            - 장애 자동 복구
            - 복구 절차 테스트
            - 수평적 확장으로 워크로드 전체 가용성 증대
            - 용량 추정 중지(오토 스케일링)
            - 변경 사항 관리 자동화 

    - **성능 효율성(Performance efficiency)**  
        클라우드 리소스를 효율적으로 사용하여 비즈니스 요구 사항을 충족하고 <u>수요 변화 및 기술 발전에 발맞춰 효율성을 유지을 유지할 수 있는 역량</u>

        - 설계 원칙
            - 고급 기술의 대중화 <sup>Democratize advanced technologies</sup>
            - 몇 분 만에 전 세계에 배포
            - 서버리스 아키텍처 사용
            - 실험 횟수 증가
            - 기계적 조화 고려<sup>Consider mechanical sympathy</sup> → 목표에 부합하는 기술 접근 방식 사용 

    - **비용 최적화(Cost optimization)**  
        워크로드 수명 주기에 걸쳐 비용을 최소화하고 ROI(투자 대비 수익률)를 최대화하여 비즈니스 가치를 제공할 수 있는 역량

        - 설계 원칙
            - 클라우드 재무 관리 구현
            - 소비 모델 채택<sup>Adopt a consumption model</sup>
            - 전반적인 효율성 측정(CloudWatch)
            - 획일적인 업무 부담에 대한 비용 지출 중단 → 데이터 센터 운영, 운영 체제 및 애플리케이션 관리 업무
            - 지출 분석 및 귀속<sup>Analyze and attribute expenditure</sup>

    - **지속 가능성(Sustainability)**  
        클라우드 워크로드 실행이 환경에 미치는 영향을 최소화  
        에너지 소비를 줄이고 효율성을 개선하기 위해 워크로드의 설계, 아키텍처 및 구현

        - 설계 원칙
            - 영향 이해
            - 지속가능성 목표 수립
            - 활용도 극대화 → 워크로드 크기를 적절하게 조정하고 효율적인 설계를 구현하여 높은 활용률을 보장
            - 새롭고 보다 효율적인 하드웨어 및 소프트웨어 제품 예측 및 채택 
            - 관리형 서비스 사용
            - 클라우드 워크로드의 다운스트림 영향 감소

#### 1.3 AWS 클라우드 마이그레이션의 이점과 전략 이해
- **AWS 클라우드 도입 <u>프레임워크</u>(AWS Cloud Adoption Framework)이란?**
    - 기본적으로 전자책이며 서비스가 아닌 규범적 지침, 백서 
    - AWS의 경험과 모범 사례에 따라 AWS를 혁신적으로 활용하여 디지털 방식으로(i.e. 클라우드를 활용하여) 비즈니스 성과를 혁신하고 가속화할 수 있도록 도움

- **AWS CAF의 6가지 관점**
    - **비즈니스(Business)**
        - 클라우드 투자가 디지털 트랜스포메이션 야망과 비즈니스 성과를 가속화하도록 보장하는 데 도움
        - 이해 관계자: `CEO`, `COO`, `CIO`, `CTO`
    - **인력(People)**
        - <u>기술과 비즈니스를 연결하는 가교 역할(as a bridge)</u>
        - 조직이 지속적인 성장과 학습의 문화로 더 빠르게 진화할 수 있도록 클라우드 여정을 가속화 
        - 이해 관계자: `COO`, `CIO`, `CTO`
    - **거버넌스(Governance)**
        - 조직의 이익을 극대화하고, 혁신 관련 위험을 최소화하면서 클라우드 이니셔티브(cloud initiatives)를 조율하는 데 도움 
        - 이해 관계자: `최고 혁신 책임자`, `CIO`, `CTO`, `CFO`, `CDO`, `CRO`

    - **플랫폼(Platform)**
        -  기업 수준의 확장 가능한 하이브리드 클라우드 플랫폼을 구축하고, 기존 워크로드를 현대화하며, 새로운 클라우드 네이티브 솔루션을 구현하는 데 도움
        - `CTO`, `기술 리더`, `설계자`, `엔지니어` 
    - **보안(Security)**
        - 데이터 및 클라우드 워크로드의 기밀성, 무결성, 가용성 달성하는데 도움 
        - 이해 관계자: `최고 정보 보안 책임자(CISO)`, `최고 규정 준수 책임자(CCO)`, `내부 감사 리더`, `보안 설계자`, `엔지니어`
    - **운영(Operations)** 
        - 클라우드 서비스가 비즈니스 요구사항을 충족하는 수준으로 제공되도록 도움 
        - 이해 관계자: `인프라 및 운영 리더`, `사이트 안정성 엔지니어`, `정보 기술 서비스 관리자`

- **AWS CAF - Transformation Domains**
    - 기술(Technology)
    - 프로세스(Process)
    - 조직(Organization)
    - 제품(Product)

- **AWS CAF - Transformation Phases**
    1. 비전 수립 단계(Envision) → 비즈니스 기회 식별
    2. 정렬 단계(Align) → 실행 계획 도출 단계(비즈니스 기회를 CAF 관점과 비교)
    3. 실행 단계(Launch)
    4. 확장 단계(Scale)

- **AWS CAF - Business Outcomes**
    - 비즈니스 위험 감소(Reduce business risk)  
    - 환경, 사회 및 거버넌스(ESG) 성과 개선(Imporve ESG performance)
    - 수익 증대(Grow revenue)
    - 운영 효율성 향상(Increased operational efficiency)

- **마이그레이션 전략(7R)**  
    - **Retire**
        - 사용 중지하거나 보관하려는 애플리케이션
    - **Retain**
        - 소스 환경에 보관하려는 애플리케이션 또는 마이그레이션할 준비가 되지 않은 애플리케이션
    - **Rehost**
        - lift and shift
        - 변경 사항 없이 소스 환경에서 AWS 클라우드로 이동 
    - **Relocate**
    - **Repurchase**
        - drop and shop
        - 애플리케이션을 다른 버전이나 제품으로 교체
    - **Replatform**
        - lift, tinker, and shift or lift and reshape
        - 애플리케이션을 클라우드로 이전하고 일정 수준의 최적화 

    - **Refactor / Re-architect**
        - <u>대규모 마이그레이션에 적절하지 않음</u>
        - 확장, 제품 및 기능 출시 가속화, 비용 절감의 비즈니스 요구가 강할 경우 

#### 1.4 클라우드 경제성의 개념 이해
