<template>
    <div :class="{ 'px-2 py-4': $vuetify.breakpoint.xs, 'px-6 py-6': $vuetify.breakpoint.smAndUp }">
        <v-breadcrumbs :items="subNavi" large class="px-0">
            <template v-slot:divider>
                <v-icon>mdi-chevron-right</v-icon>
            </template>
        </v-breadcrumbs>

        <h1 class="mb-12">계정발급</h1>

        <article class="mb-12">
            <!-- <h2 class="">계정발급</h2>
            <blockquote class="blockquote body-1">
                모든 연동작업은 가맹점 인증 요청을 통해 페이플 서버접근권한을 획득한 후에야 수행할 수 있습니다.<br />
                전체적인 가맹점 인증 프로세스에 대해서 소개해드립니다.
            </blockquote> -->

            <h3 class="pl-4">
                테스트 & 운영 계정
            </h3>
            <blockquote class="blockquote body-2">
                페이플에서는 연동테스트를 하실 수 있는 테스트 서버와 실제 운영 서버를 별도로 운영하고 있습니다.<br />
                가맹점은 테스트 환경에서 테스트 계정으로 테스트를 수행할 수 있으며<br />
                계약 이후에 발급 받게 되는 가맹점 ID(cst_id)와 가맹점 운영 Key(custKey)로 운영환경에서 실제 연동을 준비할 수 있습니다.
            </blockquote>
            <v-card class="temp_table ma-4 mb-12">
                <v-simple-table class="table-hover-disable">
                    <tbody>
                        <tr>
                            <td colspan="1" rowspan="1">
                                구분
                            </td>
                            <td colspan="1" rowspan="1">
                                테스트
                            </td>
                            <td colspan="1" rowspan="1">
                                운영
                            </td>
                        </tr>
                        <tr>
                            <td colspan="1" rowspan="1">
                                URL
                            </td>
                            <td colspan="1" rowspan="1">
                                https://testcpay.payple.kr
                            </td>
                            <td colspan="1" rowspan="1">
                                https://cpay.payple.kr
                            </td>
                        </tr>
                        <tr>
                            <td colspan="1" rowspan="1">
                                cst_id
                            </td>
                            <td colspan="1" rowspan="1">
                                test
                            </td>
                            <td colspan="1" rowspan="1">
                                계약 후 발급
                            </td>
                        </tr>
                        <tr>
                            <td colspan="1" rowspan="1">
                                custKey
                            </td>
                            <td colspan="1" rowspan="1">
                                abcd1234567890
                            </td>
                            <td colspan="1" rowspan="1">
                                계약 후 발급
                            </td>
                        </tr>
                        <tr>
                            <td colspan="1" rowspan="1">
                                취소/환불키 (PCD_REFUND_KEY)
                            </td>
                            <td colspan="1" rowspan="1" class="text-left white-space-normal word-break-break-all">
                                a41ce010ede9fcbfb3be86b24858806596a9db68b79d138b147c3e563e1829a0
                            </td>
                            <td colspan="1" rowspan="1">
                                계약 후 발급
                            </td>
                        </tr>
                        <tr>
                            <td colspan="1" rowspan="1">
                                비고
                            </td>
                            <td colspan="1" rowspan="1">
                                - 테스트환경에서는 도메인 검증을 하지 않습니다.<br/>
                                - 테스트에서 결제된 건은 실제 출금이 되지
                                않습니다.
                            </td>
                            <td colspan="1" rowspan="1">
                                - 실제 출금이 진행되며, 수수료도 차감됩니다. 최소 결제금액은 1,000원입니다.<br/>
                                - 페이플에서는 도메인 검증으로 가맹점 인증을 하고 있기 때문에 REFERER가 정상적으로 넘어오지 않을 경우 AUTH0004 에러가 발생하게 됩니다. <br/>
                                - 카페24, 가비아 등 서버호스팅 이용 시 호스팅사에 페이플 URL(테스트, 운영) 방화벽 오픈을 요청하셔야 할 수 있습니다.
                            </td>
                        </tr>
                    </tbody>
                </v-simple-table>
            </v-card>
        </article>
    </div>
</template>

<script>
import { component as VueCodeHighlight } from "vue-code-highlight";
// import vchTheme from "../../node_modules/vue-code-highlight/themes/prism-coy.css";

export default {
    components: {
        VueCodeHighlight,
    },
    data() {
        return {
            toolTipShow: false,
            code_1: `
POST /php/auth.php HTTP/1.1
Host: testcpay.payple.kr
Content-Type: application/json
Cache-Control: no-cache 
//가맹점 인증 API를 요청하는 서버와 결제창을 띄우는 서버가 다른 경우 또는 AWS 이용 가맹점인 경우 REFERER
referer: https://가맹점 도메인

{
    "cst_id": "test",
    "custKey": "abcd1234567890"
}
`,
            code_2: `
{
    "server_name” => 요청 url
    "result” => 결과확인,
    “result_msg” => 결과 메시지,
    “cst_id” => 가맹점 아이디,
    “custKey” => 가맹점 키,
    “AuthKey” => 빌링키,
    “PCD_PAY_HOST” => 결제요청 도메인,
    “PCD_PAY_URL” => 결제요청 url,
    “return_url” => 결과 리턴 url
}
`,
            code_3: `
<!-- payple js 호출. 테스트/운영 선택 -->
<scropt src="https://testcpay.payple.kr/js/cpay.payple.1.0.1.js"></scropt> <!-- 테스트 -->
<scropt src="https://cpay.payple.kr/js/cpay.payple.1.0.1.js"></scropt> <!-- 운영 -->

<!-- 가맹점 주문페이지 '결제하기' 버튼 액션 -->
<scropt>
$(document).ready( function () {
    $('#payAction').on('click', function (event) {
        var pay_work = "CERT";
        var payple_payer_id = "d0toSS9sT084bVJSNThScnFXQm9Gdz09";
        var buyer_no = "2335";
        var buyer_name = "홍길동";
        var buyer_hp = "01023457896";
        var buyer_email = "dev@payple.kr";
        var buy_goods = "상품1";
        var buy_total = "1000";
        var pay_istax = "";
        var pay_taxtotal = "10";
        var order_num = "test0553941001540967923";
        var is_reguler = "N";
        var pay_year = "";
        var pay_month = "";
        var obj = new Object();

        obj.PCD_CPAY_VER = "1.0.1";
        obj.PCD_PAY_TYPE = 'card';
        obj.PCD_PAY_WORK = pay_work;
        obj.PCD_CARD_VER = "02"
        /* (필수) 가맹점 인증요청 파일 (Node.JS : auth => [app.js] app.post('월 중복결제 방지 기능', ...) */
        obj.payple_auth_file = '월 중복결제 방지 기능/파일이름'; //절대경로 포함 파일명 (예: /절대경로/payple_auth_file)
        /* End : 가맹점 인증요청 파일 */

        /* 결과를 콜백 함수로 받고자 하는 경우 함수 설정 추가 */
        obj.callbackFunction = getResult;  // getResult : 콜백 함수명
        /* End : 결과를 콜백 함수로 받고자 하는 경우 함수 설정 추가 */

        /* 결과를 콜백 함수가 아닌 URL로 받고자 하는 경우 */
        obj.PCD_RST_URL = '/order_result.html';
        /* End : 결과를 콜백 함수가 아닌 URL로 받고자 하는 경우 */

        obj.PCD_PAYER_NO = buyer_no;
        obj.PCD_PAYER_NAME = buyer_name;
        obj.PCD_PAYER_HP = buyer_hp;
        obj.PCD_PAYER_EMAIL = buyer_email;
        obj.PCD_PAY_GOODS = buy_goods;
        obj.PCD_PAY_TOTAL = buy_total;
        obj.PCD_PAY_ISTAX = pay_istax;
        obj.PCD_PAY_TAXTOTAL = pay_taxtotal;
        obj.PCD_PAY_OID = order_num;
        obj.PCD_REGULER_FLAG = is_reguler;
        obj.PCD_PAY_YEAR = pay_year;
        obj.PCD_PAY_MONTH = pay_month;

        PaypleCpayAuthCheck(obj);
        event.preventDefault();
    });
});
</scropt>
`,
            code_4: `
POST /php/auth.php HTTP/1.1
Host: testcpay.payple.kr
Content-Type: application/json
Cache-Control: no-cache
//가맹점 인증 API를 요청하는 서버와 결제창을 띄우는 서버가 다른 경우 또는 AWS 이용 가맹점인 경우 REFERER
Referer: https://가맹점 도메인
{
    "cst_id": "test",
    "custKey": "abcd1234567890",
    "PCD_PAY_TYPE": "card"
}

<!-- 결제요청  -->
POST /php/RePayCardAct.php?ACT_=PAYM HTTTP/1.1
Host: testcpay.payple.kr
Content-Type: application/json
Cache-Control: no-cache
{
    "PCD_CPAY_VER": "1.0.1"
    "PCD_PAY_TYPE": "card",
    "PCD_PAY_WORK": “AUTH",
    “PCD_RST_URL”: “https://aaa.com/sub/fv1….”,
    "PCD_PAYER_ID": "NS9qNTgzU2xRNHR2RmFBWWFBTWk5UT09",
    "PCD_PAYER_NO": 2324,
    "PCD_PAY_GOODS": "상품1",
    "PCD_PAY_TOTAL": 1000,
    "PCD_TAXSAVE_FLAG": "Y"
}
`,
            code_5: `
{
    "result": "success",
    "result_msg": "카드등록이 완료되었습니다.",
    "cst_id": "UFVNNVZpZk4reWo5UFRualUwcGV4dz09",
    "custKey": "T3JzRkp5L1FTcEFDa1FQdHo5Um1UZz09",
    "AuthKey": "a688ccb3555c25cd722483f03e23065c3d0251701ad6da895eb2d830bc06e34d",
    " PCD_PAY_HOST ": "https://testcpay.payple.kr",
    " PCD_PAY_URL ": "/index.php?ACT_=PAYM",
    "return_url": "https://testcpay.payple.kr/index.php?ACT_=PAYM", 
}
`,
            subNavi: [
                {
                    text: "홈",
                    disabled: false,
                    to: "/",
                },
                {
                    text: "계좌이체",
                    disabled: true,
                },
                {
                    text: "환경설정",
                    disabled: true,
                },
                {
                    text: "계정발급",
                    disabled: true,
                },
            ],
        };
    },
};
</script>
