# FOSSLight Scanner

[FOSSLight Scanner](scanner)는 Prechecker, Dependency Scanner, Source Scanner, Binary Scanner 4가지의 스캐너로 구성되어 있으며, FOSSLight Scanner를 통해 4개 스캐너의 통합 결과를 생성하도록 수행할 수 있습니다.
![](scanner/images/fosslight_scanner.png)

각 Scanner에 대한 설치 및 사용 방법에 대한 가이드는 FOSSLight Scanner 하위 가이드 페이지에서 확인하실 수 있습니다.
#### FOSSLight Prechecker
[FOSSLight Prechecker](scanner/1_prechecker.md)는 소스 코드 내에 저작권 및 라이선스 규칙을 준수했는지 확인하고 또 저작권 및 라이선스, Download Location 정보를 쉽게 추가할 수 있도록 도와주는 도구로, 잘 활용할수록 불필요한 오픈소스 스캐닝을 막을 수 있습니다. 예를 들어 개발 초기부터 FOSSLight Prechecker를 활용하여 직접 개발한 소스 코드와 오픈소스 코드에 대하여 저작권 및 라이선스, Download Location 정보를 명확하게 표기하도록 관리한다면 별도의 스캐닝 작업 없이도 오픈소스 사용을 정확하게 파악할 수 있습니다.

#### FOSSLight Source Scanner
[FOSSLight Source Scanner](scanner/2_source.md)는 소스 코드 스캐닝을 수행하는 도구로, 소스 코드의 문자열을 검색하여 저작권과 라이선스 문구를 검출하는 ScanCode 와 코드 조각 스캐닝을 지원하는 scanoss 를 이용하여 오픈소스 분석을 수행합니다.

#### FOSSLight Dependency Scanner
[FOSSLight Dependency Scanner](scanner/3_dependency.md)는 여러 패키지 매니저에 대한 종속성 분석을 통하여 오픈소스 정보를 추출하는 도구로, 패키지 매니저의 Manifest 파일을 자동으로 감지하고 각 패키지 매니저별로 종속성을 분석한 후 오픈소스 정보가 포함된 보고서 파일을 생성합니다. 이때 재귀적으로 종속성 분석을 해주기 때문에, 1차 종속성만 분석하는 디펜던시 스캐너에 비해 실제 사용된 모든 오픈소스 정보를 추출할 수 있습니다.

#### FOSSLight Binary Scanner
[FOSSLight Binary Scanner](scanner/4_binary.md)는 바이너리 형태의 파일을 찾아서 바이너리 파일 목록을 추출한 후, 연계된 데이터베이스에 검출한 바이너리의 오픈소스 정보가 있다면 자동으로 오픈소스 정보를 출력해주는 도구입니다. 이는 바이너리 자체를 분석하는 방법이 아니기 때문에, 데이터베이스 정보가 많아야 바이너리 분석이 잘 수행될 수 있으니 참고하시기 바랍니다.
