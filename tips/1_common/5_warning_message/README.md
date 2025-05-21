---
sort: 5
published: true
---

# OSS Table Warning Message
OSS Table에서 Warning Message를 통해 검토가 필요한 사항을 확인할 수 있습니다.
- **Warning message 색깔별 의미**
  - <span style="color:red"><strong> 빨간색 </strong></span> : 리뷰 요청 또는 Confirm이 불가합니다. 검토 후 수정이 필요합니다.  
  - <span style="color:blue"><strong>  파란색 </strong></span> : 리뷰 요청 또는 Confirm 가능하지만, 검토가 필요한 사항입니다.  
  - <span style="color:grey"><strong> 회색 </strong></span> : 리뷰 요청이 가능한, 정보 전달을 위한 message입니다.  


## Warning message에 따른 검토 사항
{: .left-bar-title }

### 공통 
{: .specific-title }
<body>
  <div class="oss-warning-table">
    <table border="1" cellspacing="0" cellpadding="5">
      <thead>
        <tr>
          <th>Column</th>
          <th>Warning message</th>
          <th>Description</th>
          <th>검토 사항</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>OSS Name, License</td>
          <td><span style="color:red">Required</span></td>
          <td><ul><li>필수 입력 필드로, 내용 입력이 필요합니다.</li></ul></td>
          <td>-</td>
        </tr>
        <tr>
          <td>Source Path</td>
          <td><span style="color:blue">Format Warning</span></td>
          <td><ul><li>file format이 맞지 않습니다.</li></ul></td>
          <td><ul><li>파일 또는 파일의 경로가 올바르게 입력되었는지 확인합니다.</li></ul></td>
        </tr>
        <tr>
          <td>OSS Name</td>
          <td><span style="color:blue">New open source</span></td>
          <td><ul><li>FOSSLight Hub에 등록되지 않은 신규 OSS입니다.</li></ul></td>
          <td>
            <ul>
              <li>OSS List에서 비슷한 이름을 가진 OSS 중 동일한 것이 있을 경우, FOSSLight Hub에 등록된 OSS Name으로 변경합니다.</li>
              <li>동일한 OSS가 없을 경우, 수정할 필요 없습니다.이 경우 <strong>Download location, Homepage column</strong>을 필수로 기입하시기 바랍니다.</li>
            </ul>
          </td>
        </tr>
        <tr>
          <td>OSS Name</td>
          <td><span style="color:blue">Deactivated</span></td>
          <td><ul><li>FOSSLight Hub에 등록되었으나 deactivate 처리된 Legacy OSS입니다.</li></ul></td>
          <td>
            <ul>
              <li>사용된 OSS의 <strong>Download location</strong>을 필수로 기입하시기 바랍니다.</li>
            </ul>
          </td>
        </tr>
        <tr>
          <td>OSS Name</td>
          <td><span style="color:blue">Required OSS Name</span></td>
          <td><ul><li>OSS name의 입력이 필요합니다.</li></ul></td>
          <td>
            <ul>
              <li>License에 source obligation이 있는 경우 구체적인 OSS 정보가 필요합니다. OSS 출처를 확인하여 OSS Name을 기입합니다. </li>
            </ul>
          </td>
        </tr>
        <tr>
        <td>OSS Version</td>
        <td><span style="color:blue">New version</span></td>
        <td>
            <ul>
            <li>FOSSLight Hub에 등록되지 않은 신규 version입니다.</li>
            </ul>
        </td>
        <td>
            <ul>
            <li>Download location에서 해당 Source Code 다운로드 가능한지 확인합니다.</li>
            <li>
                다음과 같은 경우 version을 공란으로 표기합니다.
                <ul>
                <li>official하게 배포된 version이 아닌 경우 (ex- unspecified)</li>
                <li>version이 별도로 관리되지 않는 OSS의 경우</li>
                </ul>
            </li>
            </ul>
        </td>
        </tr>
        <tr>
          <td>License</td>
          <td><span style="color:blue">Declared : [License of OSS]</span></td>
          <td><ul><li>OSS가 FOSSLight Hub에 다른 License로 등록되어 있거나 FOSSLight Hub에 등록된 해당 OSS의 License 중 Permissive가 아닌 License Type이 누락된 경우입니다.</li></ul></td>
          <td>
            <ul>
              <li>기입된 License가 포함되는 지 또는 미작성된 License가 포함되지 않는 지 확인합니다. </li>
            </ul>
          </td>
        </tr>
        <tr>
          <td>License</td>
          <td><span style="color:blue">New license</span></td>
          <td><ul><li>FOSSLight Hub에 등록되지 않은 신규 License입니다.</li></ul></td>
          <td><ul><li>CLM을 통하여 사전에 License review 요청하기 바랍니다. </li></ul></td>
        </tr>
        <tr>
          <td>License</td>
          <td><span style="color:blue">Recommended : [License of OSS]</span></td>
          <td><ul><li>해당 OSS가 Dual License일 때, 자동완성시 입력되는 License 이외의 license가 입력된 경우입니다.</li></ul></td>
          <td>
            <ul>
              <li>더 permissive한 license를 선택하기 위해 Recommended 에 표시된 License로 변경하는 것으로 검토합니다.</li>
            </ul>
          </td>
        </tr>
        <tr>
          <td>License</td>
          <td><span style="color:red">Dual : Put one license</span></td>
          <td><ul><li>Dual License임에도 모두 사용된 것으로 쓰여져 있습니다.</li></ul></td>
          <td><ul><li>Dual License인 경우, 사용할 License를 하나만 선택해야 합니다.</li></ul></td>
        </tr>
        <tr>
          <td>License</td>
          <td><span style="color:red">Put OSS name or one license</span></td>
          <td><ul><li>OSS Name 이 - 또는 공란이면서, 여러 License가 하나의 Row에 쓰여져 있습니다.</li></ul></td>
          <td><ul><li>OSS Name 이 - 또는 공란인 경우, License별로 Row를 분리하여 작성하여주십시오.</li></ul></td>
        </tr>
        <tr>
          <td>Homepage</td>
          <td><span style="color:blue">The address should be started with www or http:// or https://</span></td>
          <td><ul><li>Homepage 주소 format이 맞지 않습니다.</li></ul></td>
          <td><ul><li>Homepage 주소를 재확인하여 www, http://, https://로 시작하는 주소로 작성합니다</li></ul></td>
        </tr>
        <tr>
          <td>OSS Version, License</td>
          <td><span style="color:red">Format error</span></td>
          <td><ul><li>줄바꿈 문자가 포함되어 있습니다.</li></ul></td>
          <td>
            <ul>
                <li>줄바꿈 문자가 포함되지 않아야합니다. </li>
                <li>여러 줄 작성이 필요한 경우, Row를 추가하여 작성하시기 바랍니다.</li>
            </ul>
          </td>
        </tr>
        <tr>
          <td>Download location, Homepage</td>
          <td><span style="color:blue">Different from DB</span></td>
          <td><ul><li>입력한 URL이 FOSSLight Hub에 등록된 URL과 다릅니다.</li></ul></td>
          <td>
            <ul>
                <li>FOSSLight Hub에 등록된 OSS와 동일한 OSS인지 검토하시기 바랍니다.</li>
                <li>다른 OSS인 경우, OSS Name을 FOSSLight Hub에 등록된 것과 구분하여 작성하여 주시기 바랍니다.</li>
            </ul>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</body>

### BIN, BIN(Android) Tab
{: .specific-title }

<body>
  <div class="oss-warning-table">
    <table border="1" cellspacing="0" cellpadding="5">
      <thead>
        <tr>
          <th>Column</th>
          <th class="warning-col">Warning message</th>
          <th colspan="2">Description</th>
          <th>검토 사항</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td rowspan="14">Binary Name</td>
          <td><span style="color:blue">Same: [OSS Name] [OSS Version] / [License]</span></td>
          <td>OSS Name, License가 다른 경우</td>
          <td rowspan="4">Binary DB에 저장된 동일한 binary에 대한 OSS 정보를 표시합니다.</td>
          <td rowspan="4">
            <ul>
              <li>Binary DB에 저장된 동일한 binary에 대한 정보를 확인 후, 필요한 경우 기재한 OSS Name / Version, License 정보를 보완합니다.</li>
            </ul>
          </td>
        </tr>
        <tr>
          <td><span style="color:blue">Same : / [License]</span></td>
          <td>License 만 다른 경우</td>
        </tr>
        <tr>
          <td><span style="color:blue">Same : [OSS Name] [OSS Version]</span></td>
          <td>OSS Name 만 다른 경우</td>
        </tr>
        <tr>
          <td><span style="color:grey">Same : [OSS Name] [OSS Version]</span></td>
          <td>OSS Version 만 다르거나, License가 동일하고 License Type이 Proprietary 또는 Proprietary Free인 경우</td>
        </tr>
        <tr>
          <td><span style="color:blue">Similar(TLSH distance) : [OSS Name] [OSS Version] / [License]</span></td>
          <td>OSS Name, License가 다른 경우</td>
          <td rowspan="4">Binary DB에 저장된 유사한 binary에 대한 OSS 정보가 표시됩니다. (괄호 안에는 TLSH distance 값이 표시됩니다.)</td>
          <td rowspan="4">
            <ul>
              <li>Binary DB에 저장된 유사한 binary에 대한 정보를 확인 후, 필요한 경우 기재한 OSS Name / Version, License 정보를 보완합니다.</li>
            </ul>
          </td>
        </tr>
        <tr>
          <td><span style="color:blue">Similar(TLSH distance) : / [License]</span></td>
          <td>License 만 다른 경우</td>
        </tr>
        <tr>
          <td><span style="color:blue">Similar(TLSH distance) : [OSS Name] [OSS Version]</span></td>
          <td>OSS Name 만 다른 경우</td>
        </tr>
        <tr>
          <td><span style="color:grey">Similar(TLSH distance) : [OSS Name] [OSS Version]</span></td>
          <td>OSS Version 만 다르거나, License가 동일하고 License Type이 Proprietary 또는 Proprietary Free인 경우</td>
        </tr>
        <tr>
          <td><span style="color:grey">Modified(TLSH distance) : [OSS Name] [OSS Version] / [License]</span></td>
          <td>OSS Name, License가 다른 경우</td>
          <td rowspan="3">Binary DB에 동일한 이름이지만 유사도가 작은 (TLSH distance > 120) binary에 대하여 OSS 정보가 회색으로 표시됩니다. (괄호 안에는 TLSH distance 값이 표시됩니다.)</td>
          <td rowspan="3">
            <ul>
              <li>Binary DB에 동일한 유사도가 작은 binary에 대한 정보를 확인 후, 필요한 경우 기재한 OSS Name / Version, License 정보를 보완합니다.</li>
            </ul>
          </td>
        </tr>
        <tr>
          <td><span style="color:grey">Modified(TLSH distance) : / [License]</span></td>
          <td>License 만 다른 경우</td>
        </tr>
        <tr>
          <td><span style="color:grey">Modified(TLSH distance) : [OSS Name] [OSS Version]</span></td>
          <td>OSS Name 또는 OSS Version 만 다른 경우</td>
        </tr>
        <tr>
          <td><span style="color:grey">Matched</span></td>
          <td>동일하거나 유사한 Binary인 경우</td>
          <td rowspan="2">Binary DB의 Binary Name, OSS Name, OSS Version, License가 일치합니다.</td>
          <td rowspan="2">-</td>
        </tr>
        <tr>
          <td><span style="color:grey">Modified(TLSH distance)</span></td>
          <td>동일한 이름이지만, 유사도가 작은 Binary인 경우</td>
        </tr>
        <tr>
          <td><span style="color:grey">New</span></td>
          <td>Binary DB에 동일한 이름의 Binary가 없습니다.</td>
          <td>-</td>
          <td>
            <ul>
              <li>Binary DB에 등록되지 않은 새로운 Binary임을 감안하여 OSS 정보 입력 시 주의합니다.</li>
            </ul>
          </td>
        </tr>
        <!-- Notice -->
        <tr>
          <td rowspan="2">Notice</td>
          <td><span style="color:blue">NOTICE should be "ok" in case OSS is used</span></td>
          <td>고지 의무가 있는 License임에도 NOTICE.html에 Binary Name column에 작성된 값이 포함되지 않은 경우입니다.</td>
          <td>-</td>
          <td>
            <ul>
              <li>사용된 Binary Name과 해당 Binary의 License text가 NOTICE.html 파일에 추가될 수 있도록 합니다.</li>
            </ul>
          </td>
        </tr>
        <tr>
          <td><span style="color:blue">Found binary in NOTICE.html</span></td>
          <td>고지 의무가 없는 License임에도 NOTICE.html에 Binary Name column에 작성된 값이 포함된 경우입니다.</td>
          <td>-</td>
          <td>
            <ul>
              <li>Open Source License가 아닌 Other proprietary license와 같이 고지되지 않아도 되는 License가 NOTICE.html에 포함되지 않도록 합니다.</li>
            </ul>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</body>
