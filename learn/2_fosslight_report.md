# FOSSLight Report

```note
FOSSLight Hub와 FOSSLight Scanner에서 사용하는 Template으로 Project별 포함하는 Open Source 현황을 파악하기 위해 작성하는 문서입니다.
```

## 양식

2.0 버전 : [fosslight_report.xlsx](https://github.com/fosslight/fosslight/raw/main/src/main/resources/template/ProjectReport.xlsx)

## Sheet 별 설명

### Project Info Sheet
![info](./images/project_info.png)
- About the report  
   작성자/부서, 작성일을 작성합니다.

- About the project  
   개발 Project에 대한 정보를 작성합니다.

- About OSC Process  
   OSC Process에 대한 정보를 작성합니다.

### 3rd Party Sheet
배포하는 Project에 3rd Party로부터 제공받은 Software가 포함되어 있다면, 3rd party로부터 OSS Checklist를 입수하여 사용한 Open Source 현황을 파악해야 합니다. (참고 : [3rd Party OSS Checklist](https://github.com/fosslight/fosslight/raw/main/src/main/resources/static/sample/FOSSLight-OSS-Checklist-for-3rdParty_Eng_1.0.xlsx))    
파악한 사항은 OSC System의 [3rd Party](../started/2_try/5_third-party.md) 메뉴에 등록하고 Identification > 3rd Party 탭에서 취합합니다.   
FOSSLight Report를 Identification > BOM 탭에서 Export하면 3rd Party 탭에 등록된 사항이 "3rd party" sheet에 채워집니다. 따라서, "3rd party" sheet는 임의로 작성하지 않습니다.    

![info](./images/3rd_party.png)

### DEP Sheet
Dependency 분석 결과를 업로드합니다.
- [FOSSLight Dependency Scanner](https://github.com/fosslight/fosslight_dependency_scanner)를 이용하면 자동으로 Dependency 분석 결과를 생성할 수 있습니다.

### SRC Sheet
Source Code 별 포함되는 Open Source 정보를 작성합니다.   

![info](./images/src.png)
- 사용한 Open Source 파일 내 License Text는 존재하지만, Open Source 이름이나, 출처가 불명확할 경우, OSS Name란에 하이픈("-")으로 작성합니다.
- 하나의 Open Source에 여러 License가 적용된 경우, License를 ,로 구분하여 작성합니다.
- 참고. [FOSSLight Source Scanner](https://github.com/fosslight/fosslight_source_scanner)를 이용하면 "SRC" sheet를 자동 생성할 수 있습니다.


### BIN Sheet
Binary 별 포함되는 Open Source 정보를 작성합니다.   

![info](./images/bin.png)
- Binary를 생성하기 위해 사용한 Open Source 파일 내 License Text는 존재하지만, Open Source 이름이나, 출처가 불명확할 경우, OSS Name란에 하이픈("-")으로 작성합니다.
- 하나의 Open Source에 여러 License가 적용된 경우, License를 ,로 구분하여 작성합니다.



### BOM Sheet
"BOM" (Bill of Materials) sheet는 FOSSLight 보고서 내 작성한 Open Source 내역을 취합한 내용을 보여주게 됩니다.      
     
이 sheet는 임의로 작성하지 않고, FOSSLight Hub의 Project에서 다운로드한 FOSSLight 보고서에 자동으로 채워집니다.

![info](./images/bom.png)






