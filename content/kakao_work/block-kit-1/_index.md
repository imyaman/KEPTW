---
weight: 1
title: Block Kit 구성 및 정책
type: docs
bookHidden: false
---

# Appendix A. 조합형 말풍선 예시

Block Kit 구성 및 정책



## A-1. Welcome 메시지

---

아래 예시는 카카오워크의 업무 도구 중 구글 캘린더 Bot을 사용자가 연결했을 때, 구글 캘린더 Bot과의 1:1 채팅방에 자동으로 전송되는 Welcome(웰컴) 메시지입니다.

[https://lh5.googleusercontent.com/2eY2wvS6uD9Jh4J1LFmXqATpNR9w_kWDaxItz8f4EsVK3iH-ESdCYKT6XJ7PTKzclW73Of7x1-XqnLsC9K3DWig3PQOE96e_FDclILf_N6MLtQQ0h0VmJdMlQlut09doSPJZGvhy](https://lh5.googleusercontent.com/2eY2wvS6uD9Jh4J1LFmXqATpNR9w_kWDaxItz8f4EsVK3iH-ESdCYKT6XJ7PTKzclW73Of7x1-XqnLsC9K3DWig3PQOE96e_FDclILf_N6MLtQQ0h0VmJdMlQlut09doSPJZGvhy)

**그림.** Welcome 메시지

```json
{
   "text": "이제 구글 캘린더 Bot으로 보다 똑똑하게, 캘린더 기능을 이용해보세요.",
   "blocks": [
       {
           "type": "image_link",
           "url": "https://sample_image.kakaowork.com/welcome-240x120jpeg"
       },
       {
           "type": "text",
           "text": "이제 구글 캘린더 Bot으로 보다 똑똑하게, 캘린더 기능을 이용해보세요. 일정 알림, 오늘의 일정 등을 Bot을 통해 쉽게 확인할 수 있습니다.",
           "markdown": false
       },
       {
           "type": "button",
           "text": "설정하기",
           "style": "default"
           "action_type": "open_system_browser",
           "value": "http://example.com/details/999"
       }
   ]
}
```

## A-2. 일정 초대 알림 메시지

---

아래 예시는 사용자가 카카오워크에서 제공하는 구글 캘린더 Bot을 사용하는 경우 발송되는 일정초대 알림 메시지입니다. 이 알림 메시지에서 하나의 긴 버튼은 버튼 블록으로, 하나의 행 안에 여러 개의 버튼을 포함하는 경우에는 Action Block으로 메시지를 구성하였습니다. 각각의 버튼을 사용자가 선택했을 때, 구글 캘린더의 일정 상세 화면으로 이동하거나 또는 참석 여부를 서버 간 통신으로 전달하는 추가적인 동작들이 일어나도록 설계되어 있습니다.

[https://lh4.googleusercontent.com/OXc2piGp7JHDDUV8W-dVV2pdn6TqYUxvK_PmqdzrTa_z8Jr6wv7ftJC7JGaJD2RODNz9i7A-l51oV4m19rwyI6GZD7MUiKtMrKMH6EGq65tAotmIMXHLykLelXZ7ERkCg3Wqeier](https://lh4.googleusercontent.com/OXc2piGp7JHDDUV8W-dVV2pdn6TqYUxvK_PmqdzrTa_z8Jr6wv7ftJC7JGaJD2RODNz9i7A-l51oV4m19rwyI6GZD7MUiKtMrKMH6EGq65tAotmIMXHLykLelXZ7ERkCg3Wqeier)

**그림.** 일정 초대 알림 메시지

**참고**
* 현재 예시에 포함된 Button Block 중 **[자세히 보기]**와 같은 링크 이동형 버튼만 제공됩니다.
* **[수락/거절/미정]** 버튼과 같이 지정된 URL로 사용자가 선택한 값이 전달되는 등의 Action에 대한 API는 추후 제공될 예정입니다.

```json
{
   "text": "다음 일정에 초대되었습니다",
   "blocks": [
       {
           "type": "text",
           "text": "*다음 일정에 초대되었습니다*",
           "markdown": true
       },
       {
           "type": "divider"
       },
       {
           "type": "text",
           "text": "*서비스 정책회의 - 기획 논의*",
           "markdown": true
       },
       {
           "type": "description",
           "term": "일시",
           "content": {
               "type": "text",
               "text": "17:00~19:00",
               "markdown": true
           },
           "accent": true
       },
       {
           "type": "description",
           "term": "장소",
           "content": {
               "type": "text",
               "text": "[판교] N9층 버드bird",
               "markdown": true
           },
           "accent": true
       },
       {
           "type": "button",
           "text": "자세히 보기",
           "style": "default"
           "action_type": "open_system_browser",
           "value": "http://example.com/details/999"
       },
       {
           "type": "divider"
       },
       {
           "type": "text",
           "text": "*참석 하시겠습니까?*",
           "markdown": true
       },
       {
           "type": "action",
           "elements": [
               {
                   "type": "button",
                   "text": "수락",
                   "style": "default"
               },
               {
                   "type": "button",
                   "text": "거절",
                   "style": "default"
               },
               {
                   "type": "button",
                   "text": "미정",
                   "style": "default"
               }
           ]
       }
   ]
}
```

## A-3. 전자결재 요청 메시지

---

아래 예시는 가상의 전자결재 서비스를 카카오워크에 Bot의 형태로 연결했을 때 발생하는 알림 메시지입니다. 알림 메시지의 수신자가 결재 유형을 한 눈에 파악할 수 있도록 컬러 스타일이 지정된 헤더 블록을 사용하고, 승인과 반려라는 각각의 버튼에 스타일을 지정하여 표현했습니다. 기안자 및 문서 넘버에는 마크다운 링크를 통하여 해당 전자결재 문서 또는 사용자 정보를 확인할 수 있는 페이지로 이동할 수 있도록 구성했습니다.

[https://lh4.googleusercontent.com/pXSRJ8zi6wBKTjZpc5mnXwVW6nqaYCX8YNKS1snIrrUlHHPY4F-NXmiSpzy096KXvaRrhz4PydUifSWkUj6GgOzqYqTUwghJQy_SlylLCKfLdECxAZDPZsprjv-dx60TZCvrK6YH](https://lh4.googleusercontent.com/pXSRJ8zi6wBKTjZpc5mnXwVW6nqaYCX8YNKS1snIrrUlHHPY4F-NXmiSpzy096KXvaRrhz4PydUifSWkUj6GgOzqYqTUwghJQy_SlylLCKfLdECxAZDPZsprjv-dx60TZCvrK6YH)

**그림.** 전자결재 요청 메시지

```json
{
   "text": "내게 요청 온 결재",
   "blocks": [
       {
           "type": "header",
           "text": "내게 요청 온 결재",
           "style": "red"
       },
       {
           "type": "text",
           "text": "*[결재요청] 회원 약관, 개인정보 처리방침 법무 검토 요청*",
           "markdown": true
       },
       {
           "type": "divider"
       },
       {
           "type": "description",
           "term": "기안자",
           "content": {
               "type": "text",
               "text": "[김지석 (Jay)](https://deeplink.sample.com)",
               "markdown": true
           },
           "accent": false
       },
       {
           "type": "divider"
       },
       {
           "type": "description",
           "term": "Type",
           "content": {
               "type": "text",
               "text": "법무 검토 요청서",
               "markdown": true
           },
           "accent": false
       },
       {
           "type": "description",
           "term": "Title",
           "content": {
               "type": "text",
               "text": "회원 약관, 개인정보 처리방침 법무 검토 요청",
               "markdown": true
           },
           "accent": false
       },
               {
           "type": "description",
           "term": "Number",
           "content": {
               "type": "text",
               "text": "[20200401-PR-0024](https://sample.link.com)",
               "markdown": true
           },
           "accent": false
       },
       {
           "type": "action",
           "elements": [
               {
                   "type": "button",
                   "text": "승인",
                   "style": "primary"
               },
               {
                   "type": "button",
                   "text": "반려",
                   "style": "default"
               }
           ]
       }
   ]
}
```




---
문서 끝. 이하 샘플





# Quondam non pater est dignior ille Eurotas

## Latent te facies

Lorem markdownum arma ignoscas vocavit quoque ille texit mandata mentis ultimus,
frementes, qui in vel. Hippotades Peleus [pennas
conscia](http://gratia.net/tot-qua.php) cuiquam Caeneus quas.

- Pater demittere evincitque reddunt
- Maxime adhuc pressit huc Danaas quid freta
- Soror ego
- Luctus linguam saxa ultroque prior Tatiumque inquit
- Saepe liquitur subita superata dederat Anius sudor

## Cum honorum Latona

O fallor [in sustinui
iussorum](http://www.spectataharundine.org/aquas-relinquit.html) equidem.
Nymphae operi oris alii fronde parens dumque, in auro ait mox ingenti proxima
iamdudum maius?

    reality(burnDocking(apache_nanometer),
            pad.property_data_programming.sectorBrowserPpga(dataMask, 37,
            recycleRup));
    intellectualVaporwareUser += -5 * 4;
    traceroute_key_upnp /= lag_optical(android.smb(thyristorTftp));
    surge_host_golden = mca_compact_device(dual_dpi_opengl, 33,
            commerce_add_ppc);
    if (lun_ipv) {
        verticalExtranet(1, thumbnail_ttl, 3);
        bar_graphics_jpeg(chipset - sector_xmp_beta);
    }

## Fronde cetera dextrae sequens pennis voce muneris

Acta cretus diem restet utque; move integer, oscula non inspirat, noctisque
scelus! Nantemque in suas vobis quamvis, et labori!

    var runtimeDiskCompiler = home - array_ad_software;
    if (internic > disk) {
        emoticonLockCron += 37 + bps - 4;
        wan_ansi_honeypot.cardGigaflops = artificialStorageCgi;
        simplex -= downloadAccess;
    }
    var volumeHardeningAndroid = pixel + tftp + onProcessorUnmount;
    sector(memory(firewire + interlaced, wired));
