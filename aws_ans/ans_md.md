# ANS Study

## 1. Amazon VPC Fundamentals

### What is VPC?

- Amazon VPC는 AWS 클라우드에서 사용자가 정의한 가상 네트워크 환경으로, 온프레미스 네트워크와 유사한 구조를 제공합니다.

- AWS 클라우드의 리소스를 가상 네트워크에 배치하고, 네트워크 설정을 세부적으로 제어할 수 있습니다.

![[vpc_mean.png]]

- 서비스의 특징
	1. 가상 네트워크 구성:
		- CIDR(Classless Inter-Domain Routing)을 사용하여 IP 주소 범위를 정의.
		- 퍼블릭 및 프라이빗 서브넷을 사용하여 네트워크를 세분화.
	2. 유연한 연결 옵션:
		- 인터넷 게이트웨이, NAT 게이트웨이, VPN, Direct Connect를 통해 외부와 통신 가능.
	3. 보안 및 제어:
		- 보안 그룹과 네트워크 ACL을 통해 세부적인 보안 규칙 설정.
		- 로컬 트래픽과 외부 트래픽에 대해 라우팅 제어.

- VPC 활용 사례
	- 온프레미스 네트워크와 연결하여 하이브리드 클라우드 환경 구성.
	- AWS 리소스 간 안전한 네트워크 통신.
	- 외부 인터넷 액세스가 필요 없는 내부 애플리케이션 배치.

- 특장점:
	- 네트워크 설정의 유연성 제공.
	- 온프레미스 네트워크와 원활하게 통합 가능.
	- AWS의 확장성을 활용하면서 보안 제어 가능.

- 유의사항:
	- 네트워크 설계가 복잡할 수 있음.
	- 잘못된 설정은 보안 문제를 유발할 수 있음.
 

### Scope of VPC with respect to AWS Account, Region & AZ

- AWS 서비스 범위
	- 글로벌 서비스: AWS 계정, IAM, Route 53과 같은 서비스는 글로벌로 작동하며, 리전 제한이 없습니다.
	- 리전 기반 서비스: EC2, RDS, VPC 등은 특정 리전 내에서만 작동하며, 데이터는 리전 간 자동 복제되지 않음(사용자 설정 필요).
	- 가용 영역(AZ): 리전 내 독립적으로 운영되는 데이터 센터. 장애 격리와 고가용성을 위해 여러 AZ 사용 권장.
	- VPC: 특정 리전 내에서 작동하며, 서브넷 및 라우트 테이블로 네트워크 트래픽 관리.

![[vpc_with_account_region_az.png]]

- 서비스 범위별 주요 사항
	- 글로벌 서비스:
		- IAM: 사용자 및 권한 관리.
		- Route 53: 글로벌 DNS 관리.
		- CloudFront: 글로벌 콘텐츠 배포.
	- 리전 기반 서비스:
		- EC2, RDS 등은 리전 내에서만 리소스를 실행.
		- 리전 간 연결 시 추가 설정 필요.
	- AZ 기반 리소스:
		- 서브넷과 EC2 인스턴스는 특정 AZ에 위치.
		- 고가용성을 위해 다중 AZ 배포 권장.
	- VPC:
		- 리전 내의 논리적 네트워크 격리.
		- 서브넷을 통해 AZ 단위로 네트워크 세분화.

- AWS 관련 서비스
	- 글로벌 서비스: IAM, Route 53, CloudFront.
	- 리전 서비스: EC2, RDS, VPC.
	- AZ 기반 서비스: 서브넷, EC2 인스턴스.

- Pros:
	- 서비스가 글로벌, 리전, AZ로 나뉘어 있어 설계가 유연함.
	- AZ를 활용한 고가용성 및 장애 격리 가능.

- Cons:
	- 리전 간 데이터 전송 시 추가 비용 발생.
	- 복잡한 리전 간 서비스 통합이 필요할 수 있음.
 

### VPC Building Blocks - Core Components

![[vpc_building_block.png]]

- CIDR (Classless Inter-Domain Routing)
	- VPC의 IP 주소 범위를 정의합니다. IPv4는 /16에서 /28 범위, IPv6는 /56 범위를 사용합니다.
	- AWS 권장 프라이빗 IP 범위:
		- 10.0.0.0/8
		- 172.16.0.0/12
		- 192.168.0.0/16

- Subnets
	- VPC를 더 작은 서브넷으로 나누어 각 가용 영역(AZ)에 배치됩니다.
	- 서브넷은 퍼블릭과 프라이빗으로 나뉘며, 퍼블릭은 인터넷 게이트웨이를 통해 인터넷과 연결됩니다.

- Route Tables
	- 서브넷의 트래픽 라우팅을 정의합니다.
	- VPC는 기본 라우팅 테이블(Main Route Table)을 갖추고 있으며, 사용자 정의 라우팅 테이블을 추가할 수 있습니다.

- Internet Gateway (IGW)
	- VPC와 인터넷 간의 통신을 지원하는 가상 게이트웨이입니다.

- Security Groups
	- EC2 인스턴스와 같은 자원에 대한 네트워크 트래픽을 제어합니다.
	- 상태 저장 방식을 사용하며, 인바운드와 아웃바운드 규칙을 설정할 수 있습니다.

- Network Access Control List (NACL)
	- 서브넷 수준에서 적용되며 트래픽을 허용하거나 차단하는 규칙을 설정합니다.
	- 상태 비저장 방식을 사용하며, 명시적으로 아웃바운드 트래픽을 열어야 합니다.

- Domain Name Server (DNS)
	- VPC의 DNS 해석을 제공합니다.
	- 기본적으로 AWS 제공 DNS 서버(Route 53 Resolver)를 사용합니다.

### VPC Addressing (CIDR)

![[vpc_cidr_mean.png]]

![[vpc_cidr_mean_2.png]]
- CIDR (Classless Inter-Domain Routing):
	- IP 주소를 할당하는 체계로, 기존의 A/B/C 클래스 방식의 단점을 보완.
	- IPv4 주소는 네트워크 주소와 호스트 주소로 구분되며, 예시: 192.168.0.0/24는 256개의 IP를 포함.
	- IPv6 주소는 글로벌 고유 주소를 제공하며, AWS에서는 /56 또는 /64 블록 크기를 사용.

- IPv4 VPC CIDR:
	- VPC 내 IP 주소 범위는 /16에서 /28까지 지원.
	- AWS 권장 프라이빗 IP 범위:
	- 10.0.0.0/8 (16,777,216개 주소)
	- 172.16.0.0/12 (1,048,576개 주소)
	- 192.168.0.0/16 (65,536개 주소)

- IPv6 VPC CIDR:
	- IPv6 주소는 AWS에서 자동 할당되며, 서브넷 크기는 /64.
	- 글로벌 고유 주소로 인터넷 연결 가능.
	- IPv6는 AWS에서 기본적으로 퍼블릭 주소로 처리됨.

- 서브넷 주소 할당:
	- VPC CIDR에서 서브넷 CIDR은 /16에서 /28 범위로 정의.
	- 각 서브넷은 AWS가 5개의 IP를 예약:
	- 네트워크 주소, 라우터, DNS 매핑, AWS 예약, 브로드캐스트 주소.

- VPC CIDR의 유연성:
	- 멀티 VPC 환경에서 중복된 CIDR을 피하기 위해 신중한 설계 필요.
	- CIDR 크기는 필요한 호스트 수에 맞게 설정하되, AWS 예약 IP를 고려.

- 주요 포인트:
	- 시험에서 CIDR 블록의 크기 및 IP 예약 규칙과 관련된 문제가 자주 등장.

| Feature		| IPv4						  | IPv6							|
| -------------- | ----------------------------- | ------------------------------- |
| Address Size   | 32-bit (4 옥텟)				 | 128-bit (8 블록)				  |
| Address Format | 점으로 구분된 10진수 (예: 192.168.1.1) | 콜론으로 구분된 16진수 (예: 2001:0db8::1) |
| Address Space  | 약 43억 개의 주소				   | 거의 무한대 (2^128 주소)			   |
| Deployment	 | 현재 가장 널리 사용됨				  | 주로 신규 네트워크 및 확장용				|
| Public/Private | 공용/비공용 구분					 | 공용만 사용 (비공용 개념 없음)			  |
| Subnet Size	| 가변적, /8 ~ /32 사용 가능		   | 고정된 서브넷 크기 (/64)				|
| Security	   | 옵션으로 IPsec 지원				 | 기본적으로 IPsec 포함				  |
| NAT 필요 여부	  | 공용 IP 부족 문제로 NAT 필요		   | 넉넉한 주소 공간으로 NAT 불필요			 |
| DNS Support	| Amazon DNS를 통해 기본적으로 지원	   | Amazon DNS로 지원, 복잡성 증가		  |
| Header Size	| 20 bytes					  | 40 bytes						|
| Compatibility  | 기존 인터넷 프로토콜과 완전 호환			| IPv4와 직접 호환되지 않음				|

### VPC Route Tables

![[vpc_route_table.png]]

- 라우트 테이블이란?
	- VPC의 서브넷이나 게이트웨이를 통해 트래픽을 라우팅하는 데 사용됩니다.
	- 라우트 테이블은 여러 서브넷과 연결될 수 있습니다.

- 기본 특징
	- VPC 생성 시 기본(Main) 라우트 테이블이 자동으로 생성됩니다.
	- 모든 서브넷은 기본적으로 메인 라우트 테이블과 연결됩니다.
	- 각 라우트 테이블은 로컬 트래픽에 대한 기본 라우트(immutable local route)를 포함합니다.

- 커스텀 라우트 테이블
	- 서브넷별 커스텀 라우트 테이블을 생성해 특정 트래픽 흐름을 설정할 수 있습니다.
	- 예: 인터넷 게이트웨이를 추가하여 인터넷에 액세스 가능하도록 설정.

- 라우트 테이블 항목
	- 대상(Destination): 트래픽이 이동할 IP 범위 (예: 0.0.0.0/0).
	- 대상(Target): 트래픽이 이동할 게이트웨이, NAT, 네트워크 인터페이스 등.

- 예시
	- 인터넷 트래픽용 라우트:
		- 대상: 0.0.0.0/0, 타겟: 인터넷 게이트웨이(IGW).
	- 온프레미스 연결:
		- 대상: 특정 IP CIDR, 타겟: 가상 프라이빗 게이트웨이(VGW).

- 장점
	- 서브넷 수준에서 세밀한 트래픽 라우팅을 지원.
	- 다양한 AWS 게이트웨이(NAT Gateway, IGW, VGW)와 통합.

- 단점
	- 잘못된 라우트 설정 시 연결 오류 발생 가능.
	- 복잡한 네트워크 설계에서는 라우트 관리가 어려울 수 있음.

#### Caution
- AWS는 각 서브넷에서 5개의 IP 주소(처음 4개와 마지막 1개)를 예약하며, 이는 인스턴스에 할당할 수 없습니다. 예를 들어, CIDR 블록이 10.0.0.0/24인 경우 다음 IP가 예약됩니다:
- 10.0.0.0: 네트워크 주소.
- 10.0.0.1: VPC 라우터에 의해 예약.
- 10.0.0.2: Amazon 제공 DNS와 매핑을 위해 예약.
- 10.0.0.3: 미래 사용을 위해 AWS에서 예약.
- 10.0.0.255: 네트워크 브로드캐스트 주소(브로드캐스트는 VPC에서 지원되지 않음).
- 시험 팁:
	- EC2 인스턴스에 필요한 IP 주소가 29개인 경우, 서브넷 크기로 /27(32개의 IP)을 선택하면 사용할 수 있는 IP가 27개밖에 되지 않아 적합하지 않습니다. 대신, /26(64개의 IP)을 선택해야 합니다.

### IP Addresses - Private vs Public vs Elastic & Allocation

![[vpc_ip_address_public.png]]

- Private IP
	- 정의: VPC의 내부 네트워크 통신용 IP 주소. 인터넷에서는 사용되지 않음.
	- 할당: EC2 인스턴스 생성 시 자동으로 할당.
	- 사용 사례: 애플리케이션 서버, 데이터베이스 서버와 같은 내부 통신 필요 리소스.

- Public IP
	- 정의: 인터넷 통신이 가능한 IP 주소. AWS IP 풀에서 할당.
	- 할당 조건: 퍼블릭 서브넷에 생성된 EC2 인스턴스가 퍼블릭 IP를 받을 수 있도록 설정.
	- 특징:
		- 인스턴스가 정지되거나 종료되면 IP가 해제.
		- 사용 사례: 웹 서버, 인터넷과 직접 통신이 필요한 리소스.

- Elastic IP (EIP)
	- 정의: 고정된 공용 IP 주소. 인스턴스에 할당하여 정적 IP로 유지.
	- 할당 및 특징:
		- 명시적으로 요청하여 AWS 계정에 할당.
		- 인스턴스가 종료되거나 정지되더라도 해제되지 않음.
		- 각 계정에 기본적으로 5개의 EIP 할당 제한.

	- 사용 사례: DNS 레코드가 변경되지 않도록 고정 IP 유지가 필요한 경우.

- 주소 재할당
	- Elastic IP는 다른 인스턴스에 재할당 가능.
	- Public IP는 인스턴스 재시작 시 새로운 주소로 변경.

- 장점
	- Private IP는 비용 효율적이며 내부 네트워크 보안을 유지.
	- Elastic IP는 고정 IP 제공으로 네트워크 설계 유연성 증가.

- 단점
	- Public IP는 정지/종료 시 변경되어 고정 IP 필요 시 Elastic IP를 사용해야 함.
	- Elastic IP는 사용하지 않을 경우에도 비용 발생.

| Feature              | Private IP               | Public IP                | Elastic IP               |
| -------------------- | ------------------------ | ------------------------ | ------------------------ |
| **Definition**       | VPC 내부 통신용 비공개 IP 주소     | 인터넷에서 직접 통신 가능한 공용 IP 주소 | 고정 공용 IP 주소, AWS 계정에 할당  |
| **Allocation**       | VPC 서브넷 CIDR 범위에서 자동 할당  | 퍼블릭 서브넷에서 자동 할당 가능       | 명시적으로 요청 후 AWS 계정에 할당    |
| **Address Range**    | RFC 1918 개인 네트워크 범위 내 주소 | AWS가 제공하는 공용 IP 풀        | AWS가 제공하는 공용 IP 풀        |
| **Persistence**      | 인스턴스 정지 시 해제되지 않음        | 인스턴스 정지/종료 시 새 주소로 변경    | 인스턴스 정지/종료에도 유지          |
| **Cost**             | 무료                       | 과금 (since 2024.2.14 ~ )  | 사용하지 않을 경우 요금 발생         |
| **Use Case**         | 내부 애플리케이션, 데이터베이스 서버     | 웹 서버, 인터넷과 통신하는 리소스      | DNS 고정 및 고정된 IP가 필요한 서비스 |
| **IP Changeability** | 변경되지 않음                  | 인스턴스 재시작 시 변경 가능         | 고정, 다른 인스턴스에 재할당 가능      |
| **Internet Access**  | NAT 또는 게이트웨이 필요          | 가능                       | 가능                       |

### VPC Firewall: Security Group

![[vpc_security_group.png]]
![[vpc_security_group_2.png]]
- Security Group이란?
	- EC2 인스턴스 및 리소스에 적용되는 가상 방화벽.
	- 인바운드(수신) 및 아웃바운드(송신) 트래픽 제어.
	- VPC 내의 인스턴스 수준에서 작동.

- 주요 특징
	- 상태 유지(Stateful):
		- 허용된 인바운드 트래픽은 자동으로 해당 연결의 아웃바운드 트래픽 허용.
		- 아웃바운드 트래픽도 동일하게 동작.

	- 기본 동작:
		- 모든 인바운드 트래픽은 차단.
		- 모든 아웃바운드 트래픽은 허용.
		- 허용 규칙만 적용 가능: 차단(Deny) 규칙은 지원하지 않음.

- 구성 요소
	- 프로토콜: TCP, UDP, ICMP 등.
	- 포트 범위: 특정 포트 또는 범위 설정.
	- 소스/대상: CIDR, IP 주소, 또는 다른 Security Group.

- 예시
	- SSH 허용:
		- 프로토콜: TCP, 포트: 22, 소스: 203.0.113.0/24.
	- HTTP 트래픽 허용:
		- 프로토콜: TCP, 포트: 80, 소스: 0.0.0.0/0.

- 참조 가능
	- 다른 Security Group을 소스로 참조 가능하여 유연한 네트워크 정책 생성.

- 장점
	- 인스턴스 단위에서 세밀한 트래픽 제어 가능.
	- 동적 소스 참조를 통해 네트워크 구성 간소화.

- 단점
	- 블랙리스트(Deny) 정책 설정 불가능.
	- 서브넷 전체가 아닌 개별 리소스에만 적용.

### VPC Firewall: Network Access Control List (NACL)

![[vpc_nacl.png]]

- NACL이란?
	- VPC의 서브넷 수준에서 트래픽을 제어하는 방화벽.
	- 서브넷에 적용되며, 서브넷에 연결된 모든 인스턴스에 영향을 미침.
	- 보안 그룹과는 달리 Stateless(상태 비저장) 동작.

- 주요 특징
	- 규칙 기반 접근 제어:
		- 허용(Allow)과 거부(Deny) 규칙 모두 지원.
		- 우선 순위 기반 규칙:
		- 규칙 번호가 낮을수록 우선 순위가 높음.
		- 규칙이 일치하면 해당 트래픽에 대해 허용 또는 거부 처리.

	- 기본 NACL 동작:
		- 새로 생성된 NACL은 모든 트래픽을 허용.
		- 기본 규칙은 삭제 가능.

- 구성 요소
	- 규칙 번호: 각 규칙에 대한 우선 순위.
	- 타입: 트래픽 유형 (예: HTTP, HTTPS, SSH).
	- 프로토콜: TCP, UDP, ICMP 등.
	- 소스/대상: 특정 IP 주소, CIDR 블록.
	- 작업: 허용(Allow) 또는 거부(Deny).

- 예시
	- SSH 액세스 차단:
		- 규칙 번호: 100, 프로토콜: TCP, 포트: 22, 소스: 0.0.0.0/0, 작업: Deny.
	- HTTPS 허용:
		- 규칙 번호: 101, 프로토콜: TCP, 포트: 443, 소스: 0.0.0.0/0, 작업: Allow.

- 장점
	- 서브넷 수준에서 전체 트래픽 차단 가능.
	- 특정 IP 주소나 범위를 기반으로 트래픽 제어.

- 단점
	- Stateless 동작으로 인해 인바운드와 아웃바운드 규칙을 각각 정의해야 함.
	- 잘못된 규칙 구성 시 전체 서브넷의 트래픽 차단 가능.

### Default VPC
![[default_vpc.png]]

Default VPC는 AWS에서 자동으로 생성되는 기본 VPC 환경으로, 다음과 같은 특징을 가지고 있습니다:

- 각 AWS 리전마다 하나의 Default VPC가 생성됩니다.
- CIDR 범위는 **172.31.0.0/16**으로 설정됩니다.
- 가용 영역(AZ)마다 `/20` 크기의 서브넷이 기본적으로 생성됩니다.
- Default VPC는 이미 **인터넷 게이트웨이(Internet Gateway)**가 연결되어 있으며, 모든 서브넷이 기본적으로 퍼블릭 서브넷으로 설정되어 외부와 통신이 가능합니다.
- 사용자는 Default VPC를 수정하거나 삭제할 수 있으며, 삭제 후에는 콘솔을 통해 재생성할 수 있습니다.

#### **장점**

- AWS에서 기본적으로 제공하므로 별도의 설정 없이 VPC 환경을 빠르게 사용할 수 있음.
- 초보자와 테스트 환경에 적합.

#### **단점**

- 기본적으로 모든 서브넷이 퍼블릭 서브넷이므로 보안상의 위험이 있을 수 있음.
- 실제 프로덕션 환경에는 적합하지 않음.

### Nat Gateway

NAT Gateway는 Amazon VPC에서 사설 서브넷에 있는 리소스가 인터넷이나 다른 AWS 서비스에 안전하게 접근할 수 있도록 지원하는 관리형 네트워크 구성 요소입니다. NAT Gateway는 들어오는 트래픽을 차단하면서 아웃바운드 트래픽만 허용합니다.

#### **특징**

- **보안 강화**: 사설 서브넷의 리소스에 대해 들어오는 트래픽을 차단하고, 아웃바운드 인터넷 접근만 허용.
- **고가용성**: NAT Gateway는 기본적으로 AZ별로 고가용성을 제공하며, 장애 복구 없이 자동으로 동작.
- **확장성**: 고성능 네트워크 처리량(최대 45Gbps)을 지원하며, 트래픽 양에 따라 자동 확장.
- **간편성**: AWS에서 관리되며, 유지보수나 설정이 간단.

![[natgw_basic.png]]


#### Multi AZ 구성
NAT Gateway의 Multi-AZ 관련 주요 사항 - NAT Gateway는 특정 가용 영역(AZ)에 배포됩니다. - 고가용성을 위해 각 AZ에 NAT Gateway를 각각 생성해야 합니다. - 각 사설 서브넷은 동일한 AZ의 NAT Gateway와 연결되도록 라우팅 테이블을 구성해야 합니다. - NAT Gateway는 AZ 수준의 장애 복구를 보장하며, 한 AZ의 NAT Gateway가 중단되더라도 다른 AZ의 NAT Gateway에는 영향을 미치지 않습니다. - 다중 AZ를 구성하면 추가적인 비용이 발생하지만, 중요한 워크로드에서 높은 가용성을 보장합니다.

#### **장점**

- AWS 관리형 서비스로 구성 및 유지보수에 부담이 없음.
- 동시 연결 및 데이터 처리량에 제한이 적어 대규모 환경에 적합.
- IP 주소 자동 할당 및 관리 기능 제공.

#### **단점**

- NAT Gateway 사용에 따른 추가 비용 발생.
- 가용 영역별로 NAT Gateway를 구성해야 하므로 설정이 단순하지만 비용이 증가할 수 있음.

#### **특이점**

- NAT Gateway는 아웃바운드 트래픽만 처리하며, 들어오는 요청은 지원하지 않음.
- 가용 영역 단위로 배포되며, 다중 AZ 아키텍처에서는 각 AZ에 하나씩 생성하는 것이 권장됨.
- IPv6에서는 NAT Gateway를 사용할 수 없으며, 대체로 Egress-only Internet Gateway를 사용.

### Nat Instance (EC2 base NAT)
#### **요약**

NAT Instance는 Amazon EC2 인스턴스를 활용하여 VPC의 사설 서브넷에 위치한 리소스가 인터넷에 접근할 수 있도록 구성하는 방법입니다. NAT Gateway와 비교하여 더 많은 사용자 정의와 비용 효율성을 제공하지만, 수동 관리가 필요합니다.

![[natinstance_demo.png]]

#### **특징**

- **EC2 기반**: NAT 역할을 수행하기 위해 Amazon EC2 인스턴스를 사용.
- **트래픽 처리**: 사설 서브넷에서 인터넷으로 나가는 아웃바운드 트래픽만 허용.
- **보안 그룹**: EC2 보안 그룹을 설정하여 네트워크 트래픽을 제어 가능.
- **유지보수 필요**: 사용자가 직접 인스턴스 크기, 소프트웨어 업데이트 및 복구를 관리해야 함.

#### **장점**

- 비용 효율적: 작은 규모의 트래픽 처리에 적합.
- 사용자 정의 가능: 고급 네트워크 설정 및 사용자 지정 소프트웨어 설치 가능.
- 다양한 선택지: EC2 인스턴스 유형을 사용자가 선택 가능.

#### **단점**

- 관리 부담: 사용자가 직접 설정 및 유지보수를 관리해야 함.
- 고가용성 부족: EC2 인스턴스 장애 시 트래픽이 중단될 수 있음.
- 성능 제한: EC2 인스턴스의 크기에 따라 트래픽 처리량이 제한됨.

#### 비교
| **특징**				| **NAT Gateway**								  | **NAT Instance**							|
|--------------------------|--------------------------------------------------|---------------------------------------------|
| **관리 주체**			| AWS 관리형 서비스								 | 사용자가 EC2 인스턴스를 관리해야 함			|
| **성능**				| 자동 확장 (최대 45Gbps)							| EC2 인스턴스 크기에 따라 성능 제한			|
| **가용성**			  | AZ별 고가용성 지원								| 인스턴스 장애 시 사용자가 수동으로 복구 필요	|
| **설치 및 설정**		 | 간단한 설정만으로 사용 가능						 | EC2 인스턴스와 관련 리소스 수동 구성 필요	   |
| **비용**				| 데이터 처리량에 따라 비용 발생					  | EC2 인스턴스 유형에 따라 비용 결정			 |
| **IPv6 지원 여부**	   | 지원하지 않음 (IPv6는 Egress-only Gateway 사용)	  | 지원하지 않음								 |
| **보안 그룹**			| NAT Gateway에 직접 보안 그룹을 적용할 수 없음		| EC2 인스턴스에 보안 그룹을 적용 가능		  |
| **유지보수**			| AWS가 자동으로 유지보수							| 사용자가 직접 인스턴스를 관리 및 유지보수 필요  |
| **다중 AZ 구성**		 | 각 AZ에 개별 NAT Gateway를 생성해야 함			   | 다중 AZ 구성 시 여러 NAT 인스턴스를 수동 구성   |
| **사용 사례**		   | 고성능, 대규모 네트워크 트래픽 처리				  | 저비용, 소규모 또는 임시 네트워크 환경		  |

#### **AWS 관련 서비스**

- **인터넷 게이트웨이(IGW)**: 인터넷으로 트래픽 라우팅.
- **라우팅 테이블**: 사설 서브넷에서 NAT Instance로 트래픽을 라우팅하도록 설정 필요.

#### **특이점**

- EC2 인스턴스에 퍼블릭 IP 주소를 할당해야 함.
- 고가용성을 위해 다중 AZ 구성 시 별도의 NAT Instance를 추가 설정해야 함.
- IP 마스커레이딩(IP Masquerading)을 통해 사설 IP 주소를 공용 IP 주소로 변환.

### VPC Exam Essentials
#### **시험 준비를 위한 주요 내용**

1. **VPC의 기본 구성 요소**:
	- CIDR 블록의 범위와 서브넷 크기 계산.
	- 기본 VPC와 커스텀 VPC의 차이점.
2. **네트워크 주소 지정**:
	- IPv4와 IPv6의 차이점.
	- 사설 IP 주소와 공용 IP 주소의 역할.
3. **보안 구성**:
	- 보안 그룹(Security Group)의 상태 유지(stateful) 특징.
	- 네트워크 ACL(Network ACL)의 상태 비유지(stateless) 특징.
	- 두 보안 기능의 차이와 사용 사례.
4. **라우팅 및 게이트웨이**:
	- 라우팅 테이블 설정 및 NAT Gateway를 통한 인터넷 액세스 구성.
	- NAT Gateway와 NAT Instance의 차이점.
5. **고가용성 아키텍처 설계**:
	- NAT Gateway의 다중 AZ 구성 방법.
	- VPC에 고가용성을 추가하는 방법.
6. **기본 VPC 환경**:
	- Default VPC의 특성과 사용 사례.
	- Default VPC 삭제 및 복구 방법.

## 2. Additionnal VPC Feature

### Extending VPC Address Space
#### **주요 개념**

1. **CIDR 블록 추가**:
	- AWS에서는 VPC에 최대 5개의 추가 CIDR 블록을 할당할 수 있음.
	- IPv4 CIDR 블록은 `/16`에서 `/28`까지 지원.
	- 추가된 CIDR은 기존 CIDR과 중복되지 않아야 함.
2. **IPv6 지원**:
	- AWS에서 할당된 IPv6 CIDR 블록은 `/56`로 고정.
	- IPv6는 전역적으로 고유하며, 프라이빗 네트워크와 퍼블릭 네트워크 모두에서 사용 가능.
3. **사용 사례**:
	- 리소스 증가로 기존 서브넷의 IP 주소가 부족할 때 CIDR 확장 필요.
	- 새로운 워크로드를 지원하기 위해 추가 서브넷을 구성해야 할 경우.
4. **제약 조건**:
	- 추가된 CIDR 블록은 원래의 VPC CIDR과 동일한 RFC 1918 범위여야 함.
	- CIDR 블록 변경은 실시간 트래픽에 영향을 미치지 않음.

#### **장점**
- 기존 리소스를 중단하지 않고 VPC 주소 공간 확장 가능.
- 유연한 네트워크 설계를 통해 리소스 증가에 대응 가능.
- IPv6 CIDR 블록을 활용하여 향후 확장성 확보.

#### **단점**
- CIDR 충돌 방지 규칙을 수동으로 관리해야 함.
- 잘못된 CIDR 설계는 복잡성을 초래할 수 있음.

![[ans/image/add_vpc_cidr.png]]![[add_vpc_cidr 1.png]]

### ENI
![[add_vpc_eni.png]]
#### **요약**
Elastic Network Interface (ENI)는 Amazon VPC의 가상 네트워크 카드로, EC2 인스턴스에 네트워크 연결을 제공하는 중요한 구성 요소입니다. ENI는 네트워크 트래픽을 유연하게 관리하고 고가용성을 지원하기 위한 다양한 기능을 제공합니다.

---

#### **주요 개념**

1. **ENI의 정의 및 역할**:
	- ENI는 VPC 내부에서 IP 주소와 네트워크 연결을 관리하는 네트워크 어댑터.
	- 인스턴스와 분리 가능하며, 한 ENI를 다른 EC2 인스턴스에 연결할 수 있음.
2. **ENI의 주요 구성 요소**:
	- **기본 NIC**: EC2 인스턴스 생성 시 자동으로 생성되며 기본 NIC로 설정.
	- **보안 그룹**: ENI 수준에서 보안 그룹을 설정하여 트래픽 제어.
	- **Private IP 및 Elastic IP**: ENI는 하나 이상의 프라이빗 IP를 가질 수 있으며, EIP와 연결 가능.
3. **ENI의 활용 사례**:
	- **다중 네트워크 구성**: 단일 인스턴스에 여러 ENI를 추가하여 다중 네트워크 연결 구성.
	- **고가용성 및 장애 복구**: 장애 시 ENI를 다른 인스턴스로 연결하여 빠른 복구 지원.
	- **애플리케이션 분리**: 한 인스턴스에서 여러 네트워크 구성을 분리하여 운영.
4. **고급 사용 사례**:
	- **ENI Trunking**: 고밀도 워크로드에서 다중 ENI를 사용하여 IP 주소 확장.
	- **ENI의 이동성**: ENI를 하나의 인스턴스에서 다른 인스턴스로 이동 가능.

---

#### **장점**

- 네트워크 구성을 유연하게 변경할 수 있음.
- 다중 네트워크를 통해 분리된 트래픽을 처리 가능.
- ENI의 분리 및 이동성을 통해 장애 복구 및 유지보수에 용이.

#### **단점**

- ENI의 설정과 관리는 사용자가 직접 수행해야 하므로 복잡성이 증가할 수 있음.
- 네트워크 처리량은 EC2 인스턴스 유형에 따라 제한될 수 있음.

---

![[add_vpc_eni_max.png]]
![[add_vpc_eni_rds.png]]
![[add_vpc_eni_dual_home.png]]
![[add_vpc_eni_ha.png]]
![[add_vpc_eni_eks_pods.png]]

### Bring Your Own IP (BYOIP)
#### **요약**

**Bring Your Own IP (BYOIP)**는 고객이 소유한 IP 주소를 AWS로 가져와 AWS 서비스와 통합해 사용할 수 있게 해주는 기능입니다. 이 기능은 클라우드로의 전환 시 네트워크 변경을 최소화하고 고객이 기존 IP 주소를 유지할 수 있도록 돕습니다.

---

#### **주요 개념**

1. **BYOIP의 정의**:
	
	- 고객이 소유한 IPv4 또는 IPv6 주소를 AWS로 이전하여 Amazon VPC 및 AWS Global Accelerator와 같은 서비스에서 사용 가능.
	- IP 주소는 AWS 리전에 매핑되어 AWS 네트워크에서 활용.
2. **주요 사용 사례**:
	
	- 기존 IP 주소를 사용하는 고객들에게 클라우드 전환 간소화.
	- 기업이 자체 IP 주소를 유지하여 DNS 변경 없이 마이그레이션 가능.
	- 규제 요건 충족을 위해 특정 IP 주소 유지.
3. **프로세스**:
	
	- **검증**: 고객의 IP 주소 소유권을 AWS에 검증.
	- **광고**: AWS의 네트워크에서 IP 주소가 광고되도록 설정.
	- **사용**: AWS 서비스에서 IP 주소를 사용(예: Elastic Load Balancer, EC2).
4. **BYOIP의 주요 제약 조건**:
	
	- AWS가 고객의 IP 소유권을 확인하기 위한 검증 절차 필요.
	- IPv4는 AWS 리전에서 BYOIP를 지원하며, IPv6은 글로벌 네트워크에서 지원.
	- IP 주소가 AWS 외부에서 사용 중인 경우 일부 제한 가능.

---

#### **장점**

- 클라우드로 전환 시 기존 네트워크 설정을 유지 가능.
- IP 주소의 소유권과 유연성을 유지하며 AWS 리소스를 활용.
- 네트워크 구성의 변경이 최소화되어 다운타임 감소.

#### **단점**

- BYOIP 설정 및 소유권 검증에는 시간이 걸릴 수 있음.
- AWS 환경 외부에서 사용하는 IP와의 통합은 추가 작업이 필요할 수 있음.

## 3. VPC DNS and DHCP
### Amazon VPC DNS 옵션

#### 요약:
- **Amazon VPC의 기본 DNS 기능:**
    
    - Amazon VPC는 기본적으로 DNS 해석 및 호스트 이름 생성 기능을 제공합니다.
    - 퍼블릭 및 프라이빗 DNS 옵션을 활성화하여 클라우드 환경에서 네트워크 자원을 효과적으로 관리.
- **DNS 호스트 이름 및 DNS 해석기:**
    
    - VPC 내에서 EC2 인스턴스에 자동으로 DNS 호스트 이름을 할당.
    - 기본적으로 `ec2.internal` 형식의 이름을 제공하며, 퍼블릭 IP가 활성화된 경우 퍼블릭 DNS 이름도 제공.
- **DNS 해석 옵션:**
    
    - 퍼블릭 DNS 해석을 허용하거나, 프라이빗 DNS만 허용하는 설정 가능.
    - AWS에서는 Route 53 Resolver를 통해 외부 DNS 쿼리를 처리할 수 있음.

#### 장점:

- 사용자가 별도로 관리할 필요 없이 자동으로 DNS가 할당.
- 프라이빗 DNS를 활용해 보안이 강화된 네트워크 환경 제공.

#### 단점:

- 기본 DNS 옵션은 커스텀 도메인 요구 사항에 적합하지 않을 수 있음.
- 퍼블릭 DNS를 활성화하면 잠재적인 보안 문제가 발생할 가능성.

#### AWS에서 관련 서비스:

- **Route 53 Resolver:** VPC와의 통합을 통해 DNS 쿼리를 처리.
- **DHCP 옵션 세트:** VPC 내 DNS 서버 주소를 동적으로 구성.

#### 주의사항:

- 퍼블릭 DNS 이름을 활성화하려면 VPC와 서브넷의 인터넷 게이트웨이가 올바르게 구성되어 있어야 함.
- 프라이빗 DNS를 사용할 때는 자체 DNS 서버를 설정하거나 AWS 제공 DNS를 사용해야 함.
### DNS VPC DNS Server (Route 53 Resolver)
![[vpc_dns_base.png]]
### Route 53 Resolver와의 통합

#### 요약:
- **Route 53 Resolver란?**
    - Amazon Route 53 Resolver는 VPC 내에서 DNS 쿼리를 처리하고 온프레미스 네트워크와의 통합을 지원하는 DNS 해석기입니다.
    - VPC와 온프레미스 네트워크 간 DNS 쿼리를 원활히 전송할 수 있도록 인바운드 및 아웃바운드 엔드포인트를 제공.
- **주요 기능:**
    - **인바운드 엔드포인트:**
        - 온프레미스 DNS 서버에서 AWS VPC로 DNS 쿼리를 전달.
    - **아웃바운드 엔드포인트:**
        - VPC 내 DNS 쿼리를 온프레미스 DNS 서버로 전달.
- **활용 사례:**
    - 온프레미스 네트워크와 AWS 간의 하이브리드 네트워크에서 통합된 DNS 관리.
    - 여러 VPC와 연결된 복잡한 네트워크 아키텍처에서 DNS 쿼리를 중앙 관리.
#### 장점:
- 하이브리드 네트워크에서 DNS 쿼리를 쉽게 통합 가능.
- VPC와 온프레미스 네트워크 간 상호작용을 단순화.
- 기존 DNS 서버를 유지하면서 AWS 네트워크를 통합할 수 있음.
#### 단점:
- 엔드포인트 설정 및 운영에 대한 추가 비용 발생.
- 잘못된 설정으로 인해 DNS 쿼리 흐름에 문제가 발생할 수 있음.
#### AWS에서 관련 서비스:
- **Route 53 Resolver:** 기본 제공 DNS 해석 및 인바운드/아웃바운드 엔드포인트.
- **VPC Peering:** 연결된 VPC 간 DNS 쿼리 처리.
- **Transit Gateway:** 여러 VPC와 온프레미스 네트워크 간 트래픽 관리.
#### 주의사항:
- 인바운드와 아웃바운드 엔드포인트는 보안 그룹을 통해 접근을 제어해야 함.
- VPC와 온프레미스 간 IP 주소 충돌이 없는지 확인 필수.
- 온프레미스 네트워크와 VPC 간 경로 설정이 정확해야 함.
#### 시험에서 자주 등장하는 패턴:
1. 하이브리드 네트워크에서 DNS 엔드포인트 구성 문제.
2. DNS 쿼리를 인바운드 및 아웃바운드 엔드포인트로 라우팅하는 시나리오.
3. Route 53 Resolver의 동작 원리를 묻는 문제.

### ### DHCP 옵션 세트

#### 요약:
- **DHCP 옵션 세트란?**
    - DHCP 옵션 세트를 통해 Amazon VPC에서 동적으로 IP 주소 및 네트워크 설정을 제공합니다.
    - 기본적으로 각 VPC는 하나의 DHCP 옵션 세트와 연결되며, 여러 네트워크 매개변수(예: DNS 서버, 도메인 이름 등)를 정의할 수 있음.
- **구성 가능한 옵션:**
    1. **Domain Name Servers (DNS):**
        - 네트워크의 기본 및 보조 DNS 서버를 지정.
    2. **Domain Name:**
        - 네트워크의 기본 도메인 이름을 설정.
    3. **NTP Servers:**
        - 네트워크 시간 동기화를 위한 NTP 서버 주소를 정의.
    4. **NetBIOS 옵션:**
        - Windows 기반 네트워크를 위한 NetBIOS 서버 및 관련 옵션을 설정.
- **활용 사례:**
    - VPC에서 커스텀 DNS 서버 사용.
    - 특정 NTP 서버로 시간 동기화.
    - 다양한 환경에 맞게 네트워크 매개변수를 동적으로 설정.

#### 장점:
- VPC 내 인스턴스가 네트워크 설정을 자동으로 수신.
- 다양한 네트워크 환경에서 쉽게 설정 변경 가능.
- 네트워크 관리 자동화로 설정 오류 최소화.

#### 단점:
- 모든 매개변수가 모든 네트워크 환경에 적용되지는 않음.
- 잘못된 옵션 설정 시 네트워크 접속 문제 발생 가능.

#### AWS에서 관련 서비스:
- **Amazon Route 53:** 커스텀 DNS 서버 설정 시 활용.
- **VPC Subnet:** 서브넷별로 DHCP 옵션 세트를 관리.

#### 주의사항:
- 기본 DHCP 옵션 세트는 삭제할 수 없으며, 필요한 경우 새 옵션 세트를 생성하여 대체.
- DHCP 옵션 세트는 VPC 단위로만 적용 가능(서브넷 단위는 불가).
- 커스텀 DNS 서버를 지정할 경우, 해당 서버가 가용한 상태인지 확인 필요.

#### 시험에서 자주 등장하는 패턴:
1. DHCP 옵션 세트를 사용하여 VPC 내 네트워크 설정 변경 문제.
2. 커스텀 DNS 서버를 사용한 구성 시나리오.
3. DHCP 옵션 세트와 기본 VPC 네트워크 매개변수의 관계를 묻는 질문.

## 4. Network Performance and Optimization

### 기초용어
	- Bandwidth : 최대 통신 가능 속도 (ex: 10mb/s, 10GB/s)
	- Latency : 지연시간
		- 단순 지연이 아닌 신호에 의한 지연일 수도 있음. 또는 연결점이 많아서 지연이 발생할 수 있음.
	- Jitter : 패킷간 지연이 변동된다.
	- Throughput : 성공적 트래픽 처리량 (ex: 10mb/s)
		- Bandwidth가 1GB/s라 해서 무조건 처리량이 1GB/s가 될 수 없으며, Latency, Jitter 등 여러가지 요소에 따라 처리량에 영향이 있을 수 있음
	- Packet per Seconds (PPS) : 초당 패킷 처리량

### Basics of Network performance - Bandwidth, Latency, Jitter, Throughput, PPS, MTU
- MTU : 일반적으로 1500Bytes (인터넷 표준)
- Jumbo Frame : MTU보다 더 큰 네트워크 패킷을 보낼 수 있음. (최대 9000bytes)
	- 장점
		- 패킷 수 감소: 점보 프레임은 표준 이더넷 프레임(1500바이트)보다 큰 패킷(최대 9000바이트)을 전송하므로, 동일한 데이터 양을 전송할 때 패킷 수가 줄어듭니다.
		- CPU 및 네트워크 부하 감소: 패킷 수가 줄어들면, 패킷당 발생하는 헤더 처리, 인터럽트, 복사 작업이 감소해 CPU와 네트워크 장치의 부하가 줄어듭니다.
		- Throughput(처리량) 증가: 패킷 수가 감소하므로, 패킷당 오버헤드가 줄어들어 네트워크의 처리량이 증가합니다. 특히 PPS(Packet Per Second) 제한이 있는 환경에서 효과적입니다.
	- 단점
		- 호환성 문제: 모든 네트워크 장치가 점보 프레임을 지원하는 것은 아닙니다. 경로 상의 장치가 점보 프레임을 지원하지 않으면 MTU(Maxi​​mum Transmission Unit) 불일치로 인해 패킷이 드롭되거나 분할됩니다.
		- 프래그멘테이션 및 재조립 비용: MTU 불일치로 인해 패킷이 쪼개지면 성능 저하가 발생합니다. 재조립 과정에서 CPU 부하가 증가할 수 있습니다.
		- 메모리 사용량 증가: 큰 프레임을 처리하려면 네트워크 카드와 드라이버가 더 많은 메모리를 필요로 합니다.
		- 에러 재전송 비용 증가: 점보 프레임에서 에러가 발생하면 큰 데이터 블록을 다시 전송해야 하므로, 재전송 비용이 증가합니다.
	- Use Case
		- Instance A -> Route 1 -> Route 2 -> Instance B
		- Instance A -(MTU 1500 DF=1)> Route 1 (OK) -(MTU 1500)> Route 2 (Not Accept 1500, Change 1000 with ICMP)
	- **MTU Path Discovery에선 ICMP가 반드시 열려 있어야 한다.**
	- AWS에선 9001 MTU를 지원하며 VPC 레벨에서 기본으로 동작한다.
	- 단, IGW, VPC Peering에선 Jumbo Frames를 지원하지 않는다. (MTU 1500)
	- DX에선 9001 MTU 지원함.
	- EC2 Cluster placement Group은 물리적으로 붙어 있어서 점보 프레임을 지원하고 최대 네트워크 Throughput을 제공한다.
	- 점보 프레임은 VPC를 떠나는 트래픽에 대해서 주의해서 사용해야 함. 패킷이 1500바이트를 초과하면 조각화되거나 IP 헤더에 조각화 안 함 플래그가 설정되어 있으면 삭제됨.
	- 점보 프레임은 ENI에서 정의됨. (tracepath amazon.com)
	- MTU 확인 하려면 **ip link show {eth name}**
	- MTU 설정을 수동으로 하려면 **sudo ip link set dev {eth name} mtu {byte number}
	- tracepath시 ec2에 public ip로 호출할 때와 private ip로 호출할때 MTU에 대한 이슈로 네트워크 성능차가 발생할 수 있음.
- AWS 내 MTU
	- VPC 레벨 : Jumbo Frames 지원 (9001 MTU)
	- VPC Endpoint : MTU 8500 byte 지원
	- IGW를 벗어나면 1500 MTU로 감소됨
	- VPC Peering이라고 하더라도 같은 리전이면 MTU 9001 지원
	- VPC Peering이 다른 리전이면 MTU 1500
- On-Premise
	- VGW를 통한 VPN : MTU 1500
	- TGW를 쓰는 STS VPN : MTU 1500
	- DX : 9001 MTU 지원
	- DX via TGW : MTU 8500 for VPC Attachment Over Direct Connect

### ENA
- 100만 이상의 PPS Performance
- 인스턴스간 Latency를 줄이는 기술
- 하이퍼바이저 out과 일관성 있는 성능을 위해 SR-IOV with PCI passthrough 적용
- Intel ixgbervf driver 또는 Elastic Network Adapter(ENA)를 사용
- SR-IOV with PCI passthrough는 가상화 수단이며 이 기술을 통해 한개의 NIC에 여러개의 네트워크가 설정된 것처럼 가상함수를 적용해서 여러개의 가상 NIC를 만들 수 있음.
- PCI가 ENI와 같은 역할을 하여 PCI상에서 여러개의 서버에 통신을 가상의 직렬형태로 연결할 수 있다.
- 조합시 대기시간이 최소화 되고 데이터 전송 속도가 증가한다.
- 전제 조건
	- 인스턴스 형식에 따라 다름
		- ixgbevf driver를 지원해야 함 (지원시 10Gbps까지)
		- Elastic Network Adapter (ENA)를 지원해야 함 (지원시 100Gbps까지)
	- 두가지 모두 충족해야 함
	- 지원 타입
		- 100Gbps: ENA 지원
			- A1, C5, C5a, C5d, C6g, F1, G3, G4, H1, I3, I3en, etc
		- 10Gbps: Intel 82599 Virtual Function 지원
			- C3, C4, D2, M4 (m4.16xlarge는 예외), R3, etc
	- EC2 Cluster Placement Group에 포함되어야 사용 가능한 옵션
	- Cluster Placement Group이 아니라면 5Gbps로 내려감.
### DPDK and Elastic Fabric Adapter (EFA)
- DPDK : Intel Data Plane Development Kit
	- SR-IOV와 향상된 네트워킹이 인스턴스와 하이퍼바이저간에 패킷 처리 부하를 감소 시킨다.
	- DPDK는 OS 내에서 벌어지는 packet 처리 부하를 감소시킨다.
![[dpdk.png]]
- EFA : ENA에 추가 되어 사용된다.
	- 낮은 Latency와 높은 throughput
	- Linux OS 전용
	- **Windows는 ENA에서 수행한다.**
	- MPI : Message Passing Interface (모든 노드 사이에서 병렬 통신이 가능하게 하여 네트워크 처리량 증가하게 한다)
	- c5n.18xlarge, p3dn.24xlarge
### Bandwidth Limits inside and outside of VPC
- NAT Gateway는 현재 최대 45Gbps까지 지원하고 있음
- 더 크게 사용한다면 멀티 NAT Gateway를 사용해야 하며 존을 넘지 않도록 할 것. 존을 넘어가면 추가 데이터 전송요금이 발생하게 됨.
- EC2의 경우 여러 요소에 의해 인스턴스 자체 대역폭을 결정하게 됨.
	- 최소
		- 인스턴스 패밀리
		- vCPU 크기
		- 트래픽 목적지
		- 네트워크 최적화 여부
		- Nitro 기반 인스턴스 여부
		- **가급적이면 인스턴스를 최신세대로 정하라.**
		- 인스턴스에서 네트워킹 대역폭도 설정할 수 있음.
		![[ec2_bandwidth_limit.png]]
		
		- 인터넷 또는 DX로 연결되는 경우 EC2가 제공하는 네트워크 대역폭의 50%만 사용 가능
		- 인스턴스 vCPU가 32보다 작으면 5GBps로 제한됨.
		- 트래픽이 igw 또는 S3 또는 다른 존으로 이동하면 50% 또는 5GBps로 대역폭이 내려간다.
	- 최대
		- Intel Virtual Function을 쓰면 최대 대역폭 10GBps를 사용가능.
		- EC2 Cluster Placement 그룹을 통해 ENA를 사용한다면 최대 대역폭 100GBps를 사용 가능
		![[ec2_max_bandwidth_ena.png]]
		- S3와 데이터 통신을 빠르게 하기 위해 S3 Endpoint를 쓰게 되면 속도를 높일 수 있다.
	- VPN and DX Bandwidth
		- VPG의 경우 총 대역폭은 25GBps이며 여러 VPN연결을 가질 수 있음.
		- 여러개와 상관없이 VPG의 총 대역폭은 25GBps임.
		- 만일 TGW라면 ECMP를 활성화하고 대역폭을 두배로 올릴 수 있음.
		- 직접 연결의 경우 포트 스피드에 의해 대역폭이 결정됨
		- **DX의 경우 1, 10, 100GBps와 50, 100, 500MBps같은 하위 속도 옵션도 제공.**
		- TGW의 경우 최대 대역폭이 50GBps.
	![[vpn_dx_bandwidth.png]]
	
### Network I/O Credit
- Network 또한 EC2 처럼 credit 시스템을 가지고 운영됨.
- R4, C5 패밀리는 network I/O Credit Mechanism으로 동작하고 있음.
- **특히 벤치마크 상황등에 해당 상황이 발생될 수 있으므로 속도가 느려지면 요런 요소도 고려할 것.**
### Summary
- 빠른 네트워크 대역폭을 처리하기 위해서 아래의 옵션을 고려할 것.
	- Jumbo Frame
	- EC2 Enhanced Networking
	- Placement groups
	- EBS Optimized Instance
	- DPDK
- Instance 레벨에서의 네트워크 최적화는
	- Enhance Networking (SR-IOV, ENA, Intel VF 82599)
	- Placement Groups
	- EBS Optimized Instance
- OS 레벨에서의 네트워크 최적화는 DPDK
- EFA는 HPC상에서 향상된 Network 성능을 위해 제공하는 OS-Bypass해주는 ENA의 추가 기능
### Exam Essential
- VPC 내에서 MTU 사이즈는 **점보 프레임**을 통해 9001 byte까지 지원한다.
- **MTU가 1500 Byte만 될때는**
	- igw를 타는 경우
	- vpc-peering이 외부 리전인 경우
	- VPN 커넥션과 연결되는 경우
- 만일 PPS가 병목에 걸려 처리량이 떨어지는 경우 MTU를 늘려서 병목을 없애야 한다.
- 네트워크 속도 향상을 위해선 **Enhanced Networking 또는 Placement Groups**를 사용할 수 있음
	- **Enhanced Networking**은 Instance <-> Hypervisor간 대기 시간을 낮춰주는 것.
	- **DPDK**를 통해서 **OS레벨에서의** 패킷 프로세싱을 더 빠르게 처리한다.
	- 인스턴스 패밀리별로 별도의 대역폭을 가지고 있음.
		- vCPU, Network Throughput, Disk I/O, Enhanced Networking 지원 여부까지도.
- 대역폭은 **다중 플로우에 걸쳐 집계된 대역폭이다.**
	- 리전 내
		- EC2의 최대 bandwidth 사용 가능
		- 리전 내에서 EC2 인스턴스 간 또는 EC2 <-> S3에 대해서 최대 100GBps까지 사용 가능 (Multi Flow)
	- 리전 밖 (igw, DX)
		- EC2의 최대 대역폭에서 50%가 떨어짐.
		- 또한 인스턴스의 vCPU가 32가 위여야 50%만 떨어지고, 32 밑이라면 50%가 추가적으로 더 떨어짐.
		- ex> 최대 20 -> igw (10) -> c5.xlarge (4vcpu) -> total 5GBps
- 같은 존간의 대역폭이 5GBps일때 Placement Groups에 배치하면 인스턴스간 10GBps까지 네트웍 속도를 올릴 수 있다. 집계 대역폭은 100GBps로 제한 (ENA)
- Endpoint와 EC2 인스턴스간에 얻을 수 있는 최대도 100GBps (ENA)

### VPC Traffic Monitoring, Trouble Shooting & Analysis

### VPC Flow Logs
- ENI의 내외부 트래픽 캡쳐
	- VPC, Subnet, ENI Level에서 감시
	- 모니터 및 Troubleshooting 목적
	- S3, Cloudwatch Logs, Kinesis Data firehose로도 보낼 수 있음
- 관리형 서비스에서 오는 네트워크 정보도 취합함.
	- ELB, RDS, ElastiCache, RedShift, Amazon Workspace, NAT Gateway, TGW 등등 
![[vpc_flow_log.png]]
- flow log version : 기본 2, 최신 5
- Flow Log는 사용자 요구에 따라 커스텀처리 할 수 있음
![[vpc_flow_log_format.png]]
- 트러블 슈팅 시나리오
![[nacl_sg_vpc_flow_log_scenario.png]]
- VPC Flow Log limit
	- DNS 서버 로그는 저장되지 않음
	- EC2 Meta Data, Time Sync, DHCP, Windows Activation, Mirroring Traffic
- 예전 툴
	- wireshark (tcpdump)
	- traceroute
	- telnet
	- nslookup : host name resolve
	- ping
### VPC Traffic Mirroring
#### 개요

- **VPC Traffic Mirroring**: AWS VPC 내에서 네트워크 트래픽을 캡처하고 복제해 모니터링 및 보안 분석에 활용하는 기능.
- **대상**: ENI(Elastic Network Interface)에 대한 트래픽을 복제.
- **주요 활용 사례**:
    - 보안 위협 탐지 및 침입 분석
    - 네트워크 성능 모니터링 및 트러블슈팅
    - 규정 준수 및 감사
---
#### 장점

- **비침투적 트래픽 캡처**: 애플리케이션 성능에 영향 없이 트래픽을 모니터링.
- **세분화된 트래픽 복제**: 특정 트래픽 필터링 가능(예: 포트, 프로토콜, IP).
- **네이티브 AWS 서비스**: 추가적인 하드웨어 구성 없이 AWS 내에서 직접 실행.
- **보안 강화**: 실시간으로 네트워크 침입 감지 및 보안 분석 가능.
- **확장성**: 필요에 따라 트래픽 복제 대상을 쉽게 확장.
---
#### 단점

- **비용**: 트래픽 양에 따라 비용이 증가.
- **복잡성**: 초기 설정 및 필터 구성에 대한 이해도가 필요.
- **대상 제한**: ENI 단위로만 트래픽을 복제 가능.
- **성능 부담**: 대규모 트래픽 복제 시 일부 성능 저하 가능.
---
#### 구동 방법

1. **Traffic Mirror Target 생성**: 트래픽을 전송할 대상(예: ENI, NLB, EC2 인스턴스 등) 생성.
2. **Traffic Mirror Filter 설정**: 복제할 트래픽의 조건(IP, 포트, 프로토콜 등)을 정의.
3. **Traffic Mirror Session 생성**:
    - 복제 대상 ENI 선택
    - 트래픽 필터 및 타겟 연결
    - 우선순위 설정
4. **트래픽 분석**: 복제된 트래픽을 보안 도구(Splunk, Wireshark 등)에서 분석.
---
### 추가 팁

- 비용 절감을 위해 특정 포트 및 IP 범위만 복제하도록 필터링 설정 권장.
- NIDS(Network Intrusion Detection System)와 연동해 보안 강화 가능.
#### VPC Traffic Mirroring – 알아두면 좋은 사항

- VPC 내부 및 외부로 흐르는 트래픽을 미러링하여 네트워크 분석 도구로 전송하고, 잠재적인 네트워크 및 보안 이상 징후를 감지할 수 있도록 함.
- **미러 소스**는 ENI(Elastic Network Interface).
- **미러 타겟**은 다른 ENI 또는 NLB(Network Load Balancer, UDP 포트 4789)일 수 있음.
- **미러 필터** – 필요한 트래픽만 캡처하도록 설정 가능.
    - 프로토콜, 소스/목적지 포트 범위, CIDR 블록을 지정해 트래픽 필터링.
    - 번호가 매겨진 규칙을 정의하여 해당 트래픽을 적절한 대상에 전송.
- 트래픽 미러 소스와 미러 타겟(모니터링 장치)은 동일한 VPC에 있거나,
    - 동일 리전의 VPC 피어링 또는 tgw를 통해 연결된 다른 VPC에 위치할 수 있음.
- 소스와 목적지는 서로 다른 AWS 계정에 있을 수 있음.
### VPC Features for Network Analysis
![[reachability_analyzer_vs_network_access_analyzer.png]]

| 항목             | Reachability Analyzer            | Network Access Analyzer           |
| -------------- | -------------------------------- | --------------------------------- |
| **목적**         | 특정 소스에서 대상까지 네트워크 경로의 연결 여부 분석   | 네트워크 경로에서 잠재적인 과도한 접근 권한을 분석 및 식별 |
| **작동 방식**      | 소스에서 대상까지 경로 추적 및 연결 상태 확인       | 다양한 경로를 분석하여 과도한 네트워크 접근을 시각화     |
| **주요 기능**      | - 특정 경로에 대한 도달 가능성 확인            | - 접근 가능한 경로 분석 및 평가               |
|                | - 네트워크 구성 문제 식별                  | - 보안 정책 위반 가능성 감지                 |
| **장점**         | - 직관적인 네트워크 경로 시각화               | - 광범위한 네트워크 분석 가능                 |
|                | - 특정 경로에 대한 세부 분석                | - 보안 및 접근 권한을 통합적으로 검토            |
| **단점**         | - 특정 소스/대상 경로에 한정                | - 과도한 경로 식별 시 분석 시간이 길어질 수 있음     |
|                | - 네트워크 접근 범위 분석은 불가능             | - 네트워크 구성 변경 사항 추적 어려움            |
| **사용 사례**      | - 두 인스턴스 간 통신 문제 해결              | - 과도한 권한 부여된 네트워크 구성 감지           |
|                | - 특정 인스턴스가 접근 불가능한 경우 경로 추적      | - 대규모 네트워크 접근 정책 감사 및 리포트         |
|                | - 방화벽 규칙 및 보안 그룹 구성 검토           | - 네트워크 보안 태세 개선 및 규정 준수 확인        |
| **탐지 가능한 리소스** | - ENI(Elastic Network Interface) | - ENI(Elastic Network Interface)  |
|                | - EC2 인스턴스                       | - EC2 인스턴스                        |
|                | - 인터넷 게이트웨이                      | - 인터넷 게이트웨이                       |
|                | - NAT 게이트웨이                      | - NAT 게이트웨이                       |
|                | - Transit Gateway                | - Transit Gateway                 |
|                | - VPC 피어링                        | - VPC 피어링                         |
|                | - 보안 그룹(Security Group)          | - 보안 그룹(Security Group)           |
|                | - 네트워크 ACL                       | - 네트워크 ACL                        |
|                | - AWS PrivateLink                | - AWS PrivateLink                 |
|                | - 로드 밸런서(ALB/NLB)                | - 로드 밸런서(ALB/NLB)                 |
|                |                                  | - S3 버킷 정책                        |
|                |                                  | - Lambda 함수 정책                    |
|                |                                  | - IAM 정책                          |

## 5. VPC Peering
### VPC Peering is?
- AWS 네트워크를 이용해 VPC 2개를 연결하는 과정이며 동일한 네트워크를 사용하는 효과를 가지게 하는 것이다.
- AWS의 다른 계정에도 사용할 수 있고 다른 리전의 VPC에도 연동이 된다.
- CIDR이 겹치지 않아야 한다.
### Step
1. Create VPC-A (10.10.0.0/16)
2. Attach igw with VPC-A
3. Create Public Subnet in VPC-A (10.10.0.0/24)
4. Create EC2 in Public Subnet and Assign IP and Open Port 22
5. Create VPC-B (10.20.0.0/16)
6. Create Private Subent in VPC-B (10.20.0.0/24)
7. Create EC2 in Private Subnet and Open Port 22, ICMP open in VPC-A CIDR
8. Create VPC Peering VPC-A to VPC-B
9. Accept Connect request in VPC-B
10. Modify Route Table in both side for Traffic on other VPC
11. Login EC2-A Instance from EC2-B (ping or SSH)
![[vpc_peering.png]]
### Limit
- CIDR 겹치면 안됨
- VPC Peering은 2개의 VPC 피어링에 대해서만 허용한다.
- 링크 형태로 물려있다고 해서 통신이 되진 않는다.
- VPC당 최대 25개의 Peering Connection만 허용된다.
![[vpc_peering_limit.png]]
### VPC Peering으로 통신이 안되는 사례
![[vpc_peering_error1.png]]![[vpc_peering_error2.png]]
## 6. VPC Gateway Endpoint
### 개요
- Private Network 상황에서 다른 AWS Service를 사용할 수 있게 한다.
- AWS 타 서비스 연결 시, IGW, NAT Gateway가 필요하지 않다.
- 인터넷 연결을 피함으로서 네트워크 요금을 감면할 수 있다.
- 고가용성이며 별도의 트래픽 제한이 없다.
- **Gateway Endpoint : Route Table을 통해서 연결이 가능 (S3, DynamoDB)**
- **Interface Endpoint : ENI를 통해서 연결이 가능 (기타 AWS Service)**
![[vpc_endpoint_type.png]]
### Gateway Endpoint
- S3, DynamoDB가 VPC의 Private connect에서 연결될 수 있도록 한다.
- Route Table에서 S3와 DynamoDB의 Gateway Endpoint가 연결될 수 있도록 해야 한다.
- S3 엔드포인트가 만들어지면 prefix 리스트가 VPC에 생성된다.
- prefix 목록은 S3를 사용하는 IP의 집합 (pl-xxxxxx 형태)
- 라우팅 테이블 및 Security Groups에서도 사용이 가능하다.
![[vpc_gateway_endpoint_ex1.png]]
![[vpc_gateway_endpoint_ex2.png]]
- 보안
	- VPC Endpoint 정책에 특정 Bucket만 접근 허용도 가능하다.
	- IAM과 권한 처리가 유사함.
	- 특정하는 S3 Bucket, DynamoDB Table 모두 가능
![[Pasted image 20250105115618.png]]
- 정책
	- 리소스 기반정책 설정이 가능
	![[vpc_gateway_endpoint_pol1.png]]
	
	![[vpc_gateway_endpoint_pol2.png]]
	
- S3에선 이렇게 설정이 가능.
	- aws:sourceVpce 형태로 vpc endpoint가 접근하는 부분에 대해서 제어가 가능함.
	- aws:sourceVpc의 경우, 특정 VPC만 차단함.
	- 필요한 경우 Public IP로도 제한이 가능하고, Private IP는 처리할 수 없음.
![[vpc_gateway_endpoint_pol_from_s3.png]]
- Steps
	1. VPC 생성 후 Public/Private Subnet 생성
	2. 각 서브넷에 EC2 생성
	3. Private EC2의 IAM role에 S3 접근 권한 부여
	4. VPC Gateway Endpoint for S3 생성
	5. Private Subnet의 Route Table에 S3 Gateway Endpoint 수정
	6. Public EC2에서 ssh로 Private EC2로 접근
	7. S3 파일이 업로드 되는지 확인
- Case
	- EC2 -> S3로 데이터를 보낼 때 막힌다?
	- IAM Role에 막힌 게 있는지 확인할 것.
	- Security Group에 VPC Endpoint가 열려 있는지 확인할 것.
	- Route Table에 VPC Endpoint가 있는지 확인할 것.
	- VPC Endpoint 정책에 문제가 있는지 확인할 것.
	- S3 Bucket Policy에 문제가 있는지 확인할 것.
	![[vpc_gateway_endpoint_pol_case.png]]
### VPC Gateway Endpoint Access From Remote Network
![[vpc_gateway_endpoint_remote_ok.png]]

## 7. VPC Interface Endpoint and PrivateLink
### VPC Interface Endpoint
- ENI를 이용해서 VPC에 접근
- Interface Endpoint는 AZ 별로 배치해야 한다.
- **0.01$/hr per AZ**
+ **0.01$/GB**
+ Security Group을 사용하므로 inboud rule을 항상 체크해야 함.
+ 지역별 DNS를 사용하여 IP 선택할 수 있지만 특정 ENI를 선택은 불가능하다.
+ 현재는 IPv4만 지원하고 있고 프로토콜은 TCP만 지원 가능.
+ Route 53이 UDP를 지원하므로 Interface Endpoint를 통해 DNS를 체크하는 것이 아니기 때문에 TCP만 제공해도 현재는 큰 문제가 없음.
### VPC PrivateLink
- 보안에 좋고 1000개 가까운 VPC에 연동이 가능 (다른 계정 포함)
- VPC Peering, igw, NAT gw, route table 모두 불필요함.
- 대신 NLB (Service VPC)와 ENI (Customer VPC)가 필요함
![[vpc_private_link.png]]
![[vpc_private_link_architecture.png]]![[vpc_private_link_architecture_onpremise.png]]
![[vpc_private_link_demo.png]]

- **사전 준비 사항** – httpd 웹 서버를 포함한 EC2 AMI를 생성합니다. 이를 사용하여 VPC-B의 Private EC2 인스턴스를 실행해 더미 서비스를 호스팅합니다.
- VPC-B를 생성하고 2개의 Private 서브넷을 설정합니다.
- 이전에 생성한 AMI를 사용하여 Private 서브넷에 EC2 인스턴스를 실행합니다.
- 다른 Private 서브넷에 NLB(Network Load Balancer)를 생성하고, NLB 뒤에 EC2 인스턴스를 등록합니다.
- VPC-B에서 VPC Endpoint Service를 생성하고 NLB를 연결합니다.
- VPC-A와 VPC-B가 서로 다른 AWS 계정에 있는 경우, VPC-A의 AWS 계정을 허용 목록(Whitelist)에 추가합니다.
- Public 및 Private 서브넷을 포함한 Service Consumer VPC(VPC-A)를 생성합니다.
- VPC-A에서 VPC Endpoint를 생성하고, 위에서 생성한 Endpoint Service를 검색합니다.
- Consumer VPC의 Private EC2 인스턴스에 로그인하고 VPC Endpoint DNS에 접근합니다.

### VPC Interface Endpoint - DNS
- private endpoint interface hostname을 가진 ENI를 배포
- interface endpoint용 private DNS 세팅
	- 서비스의 public hostname이 private hostname으로 IP Resolve 해줄 것이다.
	- VPC 세팅: "Enable DNS Hostnames"와 "Enable DNS Support"가 꼭 켜져 있어야 한다.
- Private DNS가 켜져 있으면, 소비자 VPC에는 
	- Public Pattern: servicename.region.amazonaws.com
	- Private Pattern: vpce-12345.ab.ec2.us-east-1.vpce.amazonaws.com
![[vpc_interface_endpoint_dns.png]]
![[vpc_private_link_disabled.png]]

- Subnet 1 -> kinesis.us-east-1.amazon.com -> igw
- Subnet 1 (no igw) -> vpce-123-ab.kinesis.us-east-1.vpce-amazonaws.com -> interface endpoint
- Subnet 2 (no igw) -> <<Interface Endpoint DNS를 쓴다는 조건>> interface endpoint -> kinesis

### VPC Interface Endpoint from 원격 네트워크
![[vpc_interface_endpoint_remote.png]]

- VPC에는 당연히 정상적으로 동작함.
- On-Premise는 직접 해석이 불가능 하므로 Custom Route 53 Resolver를 사용하여 해결해야 함.

### VPC PrivateLink vs VPC Peering

|        | VPC Peering                     | VPC PrivateLink                        |
| ------ | ------------------------------- | -------------------------------------- |
| 연결성    | Peered된 VPC간의 여러 리소스와 연동이 가능하다. | 타 VPC의 특정 어플리케이션만 연결할 수 있다.            |
| 연결 제한  | CIDR이 중복된 경우, 연결할 수 없다.         | CIDR이 중복되더라도 상관없이 연결할 수 있다.            |
| 연결 max | 최대 125개 연결 Peering              | 제한 없음.                                 |
| 양방향 통신 | Peering이 되고 나면 양방향 통신이 허용됨      | 단 방향만 통신 가능. 즉 A->B는 되지만, B->A는 되지 않음. |
### Summary
- **VPC 피어링**은 동일 리전 또는 서로 다른 리전의 두 VPC 간 통신을 가능하게 합니다.
- **VPC 엔드포인트**는 인터넷 게이트웨이, NAT 장치, VPN 연결 또는 AWS Direct Connect 연결 없이, PrivateLink를 통해 지원되는 AWS 서비스 및 기타 서비스를 VPC와 안전하게 연결할 수 있도록 합니다.
- VPC 내 인스턴스는 서비스 내 리소스와 통신하기 위해 공인 IP 주소가 필요하지 않습니다.
- VPC와 다른 서비스 간 트래픽은 Amazon 네트워크를 벗어나지 않습니다.
- **게이트웨이 VPC 엔드포인트**는 서비스를 연결할 수 있게 해주지만 VPC 내 네트워크 구성 요소로 존재하지는 않습니다.
- **인터페이스 VPC 엔드포인트**는 탄력적 네트워크 인터페이스, 비공개 IP 주소, DNS 이름으로 구성되며, 이를 통해 VPC 내부에서 AWS 클라우드 서비스를 안전하게 액세스할 수 있습니다.
- **AWS PrivateLink**는 인터페이스 VPC 엔드포인트의 확장 기능으로, 사용자가 자신의 엔드포인트를 생성하거나 다른 사용자가 생성한 엔드포인트를 사용할 수 있도록 합니다.

### Exam Essential
- **VPC 피어링**은 전달 라우팅(transitive routing)을 지원하지 않습니다. 따라서 피어링 연결을 통해 다른 VPC의 IGW(인터넷 게이트웨이)나 NAT에 접근할 수 없습니다.
- VPC 피어링은 다른 VPC의 보안 그룹(Security Group)을 인바운드 규칙의 소스로 사용할 수 있습니다.
- 최대 125개의 VPC 피어링 연결을 생성할 수 있습니다.
- **VPC Gateway Endpoint**는 IGW(인터넷 게이트웨이)나 NAT 없이 동일 AWS 리전에서 S3 또는 DynamoDB와의 비공개 연결을 가능하게 합니다.
- VPC Gateway Endpoint를 통해 트래픽을 라우팅하려면 필요한 서브넷의 라우팅 테이블을 수정해야 합니다.
- VPC Gateway Endpoint는 Direct Connect/VPN 또는 VPC 피어링을 통해 액세스할 수 없습니다.
- **VPC 인터페이스 엔드포인트**는 서브넷에 ENI(탄력적 네트워크 인터페이스)를 생성합니다.
- 인터페이스 엔드포인트는 리전 및 영역 DNS 이름을 수신합니다.
- **Route53 프라이빗 호스팅 존**을 사용하여 인터페이스 DNS에 대한 Alias 레코드를 활용해 커스텀 DNS를 사용할 수 있습니다.
- 인터페이스 엔드포인트는 **Direct Connect 연결**, AWS 관리형 VPN, 그리고 VPC 피어링 연결을 통해 액세스할 수 있습니다.
- 트래픽은 VPC 내 리소스에서 시작되며, 엔드포인트 서비스는 요청에만 응답할 수 있습니다. 엔드포인트 서비스가 요청을 시작할 수는 없습니다.

## 8. Transit Gateway
![[transit_gateway.png]]
- 천개 이상의 VPC 또는 On-Premise 네트워크와 연동가능
	- 1개 이상의 VPC
	- TGW와도 연동 가능
	- SD-WAN/3rd party network appliance와도 연동 가능
	- VPN
	- Direct Connect Gateway
- Multicast Support, MTU, Appliance Mode, AZ 고려, 계정 간 TGW 공유
![[tgw_sharing.png]]
![[tgw_connect_vpn.png]]
![[tgw_connect_direct_connect.png]]![[image/tgw_attachments.png]]
### TGW Attachments
![[tgw_attachments.png]]

### TGW VPC Network Pattern
- **Flat Network** : TGW에 VPC가 평면으로 붙은 hub & spoke의 전형적인 모형
	- TGW Routing table의 경우 전체 연결이 가능하며 Routing 도메인이 하나다.
![[vpc_net_pat_flat.png]]
- **Segmented Network**: Flat 구조 이외에 VPN이 붙는다던지 한다면 기본 AWS 네트워크와 VPN 연결을 구성한다.
	- VPC간은 통신을 안하지만 모든 VPC가 VPN과 통신이 가능하고 VPN측 추가 라우팅에서 VPC와 IP대역을 연결하면 통신이 가능하게 한다.
![[vpc_net_pat_segmented.png]]
### VPC & Subnet Design for TGW
- VPC를 TGW에 연결할 때, TGW가 VPC 서브넷의 리소스로 트래픽을 라우팅할 수 있도록 하나 이상의 가용 영역(Availability Zone)을 활성화해야 합니다.
- 각 가용 영역을 활성화하려면 정확히 하나의 서브넷을 지정해야 하며(일반적으로 워크로드 서브넷을 위해 IP를 절약할 수 있는 /28 범위 사용),
- TGW는 해당 서브넷에 네트워크 인터페이스를 배치하며 서브넷에서 하나의 IP 주소를 사용합니다.
- 가용 영역을 활성화한 후에는 해당 영역의 모든 서브넷으로 트래픽을 라우팅할 수 있으며, 지정된 서브넷에만 한정되지 않습니다.
- TGW 연결이 없는 가용 영역에 위치한 리소스는 TGW에 접근할 수 없습니다.
### TGW AZ Affinity & Appliance Mode
![[tgw_az_inffinity.png]]
- TGW는 목적지 트래픽에 도달할때까지 같은 AZ간의 통신으로 유지하려고 한다.
	- 요금 절감
	- 트래픽 이동 적음
	- 만일 AZ가 다운되도 피해가 적음.
![[tgw_az_inffinity_other_az.png]]
- 만일 통신하는 존이 다르다면 비대칭 라우팅이라 한다.
- **네트워크 어플라이언스 상태가 달라서 어플리케이션에서 트래픽 일치 여부를 체크할 경우 문제가 될 수 있다. 아래처럼 어플라이언스에서 트래픽을 삭제처리 할 수 있으므로 어플라이언스 모드를 활성화해야 함.**
	- **Flow Hash Algorithm**을 이용해서 반환 트래픽에 대해서 Appliance 서버의 AZ가 달라도 그 방향으로 넣어주도록 조정한다.
	- **5 tuples**: Source Port, IP, Dest Port IP, Protocol을 이용함.
![[vpc_az_appliance_mode_off.png]]
![[vpc_az_appliance_mode_on.png]]
### TGW Peering
- TGW는 리전 기반 라우터로, 동일한 리전 내에서 VPC를 연결할 수 있습니다.
- **리전 간 네트워크 연결**을 위해 트랜짓 게이트웨이를 서로 피어링할 수 있습니다.
- 피어링 연결을 위해 **Static Routes**를 추가해야 하며, **BGP는 지원되지 않습니다.**
- **리전 간 트래픽**은 암호화되어 AWS 글로벌 네트워크를 통해 전송되며, 공용 인터넷에 노출되지 않습니다. 최대 **50 Gbps** 대역폭을 지원합니다.
- 피어링된 TGW에는 **고유한 ASN**을 사용하는 것이 좋습니다 (가능한 경우).
![[tgw_peer_1.png]]
![[vpc_peer_2.png]]
### TGW Connect Attachment
- **Transit Gateway Connect Attachment**를 생성하여 트랜짓 게이트웨이와 VPC에서 실행 중인 서드파티 가상 어플라이언스(SD-WAN 어플라이언스 등) 간의 연결을 설정할 수 있습니다.
- **Connect Attachment**는 기존 VPC 또는 AWS Direct Connect Attachment를 기본 전송 메커니즘으로 사용합니다.
- **GRE(Generic Routing Encapsulation) 터널 프로토콜**을 통해 고성능 전송을 지원하며, **BGP(Border Gateway Protocol)**를 사용해 동적 라우팅을 설정합니다.