```js
<template>
  <div class="voucher-main" style="width: 1120px;margin: 0 auto;">
    <div
      v-show="voucherInfo.status !== 0"
      class="voucher_status"
      :class="{
        hasAudit: voucherInfo.status === 2,
        hasCheckOut: voucherInfo.status === 1
      }"
    ></div>
    <div
      v-if="view !== 'input'"
      class="voucher_end_status"
      :class="{
        hasFixed: voucherInfo.addWay === 9,
        hasInvisible: voucherInfo.addWay === 5,
        hasAssets: voucherInfo.addWay === 8
      }"
    ></div>
    <el-form
      v-if="type !== 'template'"
      :inline="true"
      :model="voucherInfo"
      size="small"
      class="voucher-header"
    >
      <el-form-item label="记 -" class="account_header_code">
        <span class="record_span">
          <input
            maxlength="4"
            tabindex="-1"
            :disabled="!canEdit"
            class="voucher-number"
            @keyup="numberKeyEvent"
            @input="inputVoucherNumber($event)"
            v-model="voucherInfo.number"
          />
          <span
            class="add_record"
            @click.stop="changeVoucherNumber('add')"
          ></span>
          <span
            class="reduce_record"
            @click.stop="changeVoucherNumber('reduce')"
          ></span>
        </span>
        <span class="record_num">号</span>
      </el-form-item>
      <el-form-item label="日期" class="account_header_date">
        <el-date-picker
          type="date"
          :disabled="
            !canEdit || voucherType !== 0 || !!showAssitAdd || showAddSubject
          "
          :editable="false"
          :clearable="false"
          :picker-options="pickerOptions"
          v-model="voucherInfo.date"
          value-format="yyyy-MM-dd"
          placeholder="选择日期"
        ></el-date-picker>
      </el-form-item>
      <el-form-item
        label="附单据数"
        style="float: right; width: 250px !important;"
        class="account_header_bill"
      >
        <span class="bill_span">
          <input
            maxlength="4"
            tabindex="-1"
            :disabled="!canEdit"
            class="voucher-number"
            @keyup="attachmentCountKeyEvent"
            @input="inputAttachmentCount($event)"
            @blur="attachmentCountBlur($event)"
            v-model="voucherInfo.attachmentCount"
          />
          <span
            class="add_bill"
            @click.stop="changeAttachmentCount('add')"
          ></span>
          <span
            class="reduce_bill"
            @click.stop="changeAttachmentCount('reduce')"
          ></span>
        </span>
        <span class="bill_num">张</span>
        <span class="bill_num bill-curser" @click="uploadBouncedIsShow"
          ><i class="el-icon-paperclip"></i>附件</span
        >
        <account-upload-bounced
          ref="accountUploadBounced"
          v-show="isShowUploadBounced"
          class="upload-bounced"
          :type="type"
          :upladFile="upladFile"
          :uploadFiles="uploadFiles"
          @getUploadFile="getUploadFile"
        />
      </el-form-item>
      <el-form-item style="float: right">
        <div class="cur_date">{{ currentPeriod }}</div>
      </el-form-item>
    </el-form>
    <div class="voucher-body">
      <el-row class="voucher-body-title">
        <el-col :span="1">
          <div class="grid-content"></div>
        </el-col>
        <el-col :span="disgestColSpan">
          <div class="grid-content">摘要</div>
        </el-col>
        <el-col :span="accountColSpan">
          <el-row class="account">
            <el-col :span="innerAmountColSpan">
              <div class="grid-content">会计科目</div>
            </el-col>
            <el-col :span="qtyColSpan" v-if="hasQty">
              <div class="grid-content">数量</div>
            </el-col>
          </el-row>
        </el-col>
        <el-col :span="amountColSpan">
          <div class="grid-content">
            <div class="amount-title">借方金额</div>
            <AmountName />
          </div>
        </el-col>
        <el-col :span="amountColSpan">
          <div class="grid-content last-amount-grid">
            <div class="amount-title">贷方金额</div>
            <AmountName />
          </div>
        </el-col>
        <el-col :span="1">
          <div class="grid-content"></div>
        </el-col>
      </el-row>
      <div class="voucher-body-rows">
        <div
          v-for="(detail, index) in voucherDetail"
          :key="index"
          class="voucherRow el-row"
          :class="{ canEdit }"
        >
          <el-col :span="1">
            <div class="grid-content add-row-button">
              <i class="icon-icon-add-top" @click="addRowToIndex(index)"></i>
              <span class="indexArea">{{ index + 1 }}</span>
              <i
                class="icon-icon-add-bottom"
                @click="addRowToBottom(index)"
              ></i>
            </div>
          </el-col>
          <el-col :span="disgestColSpan">
            <digest-select
              class="grid-content"
              :ref="`digest${index}`"
              :default-select="lastDigest"
              :value="detail.digest"
              :width="179"
              :disabled="!canEdit"
              :editable="!canEdit"
              @input="changeDigest($event, detail, index)"
              @delete-option="handleDeleteOption"
              @after-select="enterDigest($event, detail, index, false, true)"
              @after-select:not-confirm="
                enterDigest($event, detail, index, false, false)
              "
              @tab="enterDigest($event, detail, index, true, true)"
            />
          </el-col>
          <el-col :span="accountColSpan">
            <el-row class="grid-content">
              <el-col :span="innerAmountColSpan">
                <account-select
                  class="account-select"
                  :ref="`account${index}`"
                  v-show="detail.showAccount"
                  :value="detail.account"
                  :assist="detail.assist"
                  :width="accountWidth"
                  :disabled="!canEdit"
                  :editable="!canEdit"
                  :hover="detail.active"
                  :balance="detail.balance"
                  :status="voucherInfo.status"
                  :date="voucherInfo.date"
                  @close-modal="closeModal"
                  @add-account="addAccount(index)"
                  @input="changeAccount($event, detail, index)"
                  @after-select="enterAccount($event, detail, index)"
                  @after-select:not-confirm="
                    enterAccount($event, detail, index)
                  "
                  @tab="enterAccount($event, detail, index, true)"
                  @visible-change="accountVisibleChange($event, detail, index)"
                />

                <assist-select
                  class="assist-select"
                  :ref="`assist${index}`"
                  v-show="detail.showAssist"
                  :value="detail.assist"
                  :account="detail.account"
                  :width="accountWidth"
                  :disabled="!canEdit"
                  @add-assist="addAssist(index, detail)"
                  @input="changeAssist($event, detail, index)"
                  @after-select="enterAssist($event, detail, index)"
                  @after-select:not-confirm="enterAssist($event, detail, index)"
                  @tab="enterAssist($event, detail, index, true)"
                  @visible-change="assistVisibleChange($event, detail, index)"
                />
              </el-col>
              <el-col class="qty-content" :span="qtyColSpan" v-if="hasQty">
                <template v-if="detail.isQuantity">
                  <div class="account-qty">
                    <span class="qty-label">数量</span>
                    <input
                      :ref="`accountQty${index}`"
                      :disabled="!canEdit"
                      @input="
                        accountQtyInput($event.target.value, detail, index)
                      "
                      @focus="accountQtyFocus($event, detail, index)"
                      @blur="blurAccountQty($event.target.value, detail, index)"
                      @keydown.enter="enterAccountQty($event, detail, index)"
                      @keydown.tab="
                        enterAccountQty($event, detail, index, true)
                      "
                      type="text"
                      v-model="detail.qty"
                    />
                    <span :title="detail.unit" class="qty-unit">{{
                      detail.account.unit
                    }}</span>
                  </div>
                  <div class="account-qty">
                    <span class="qty-label">单价</span>
                    <input
                      :ref="`accountPrice${index}`"
                      type="text"
                      :disabled="!canEdit"
                      @input="
                        accountPriceInput($event.target.value, detail, index)
                      "
                      @focus="accountPriceFocus($event, detail, index)"
                      @blur="
                        blurAccountPrice($event.target.value, detail, index)
                      "
                      @keydown.enter="enterAccountPrice($event, detail, index)"
                      @keydown.tab="
                        enterAccountPrice($event, detail, index, true)
                      "
                      v-model="detail.price"
                    />
                  </div>
                </template>
              </el-col>
            </el-row>
          </el-col>
          <el-col :span="amountColSpan">
            <amount-unit
              :ref="`debitAmount${index}`"
              class="grid-content"
              :type="'value'"
              :active="activeAmountUnit"
              :editable="!canEdit"
              @enter="enterDebitAmount($event, detail, index)"
              @tab="enterDebitAmount($event, detail, index, true)"
              @native="enterDebitAmount($event, detail, index)"
              @input="amountInput('debit', detail, index)"
              @top="amountTopPriceFocus('debit', $event, detail, index)"
              @bottom="amountBottomPriceFocus('debit', $event, detail, index)"
              @type-equal="onTypeEqual('debit', detail, index)"
              @type-blank="onTypeBlank('debit', detail, index)"
              @closeOther="closeOther"
              v-model="detail.debitAmount"
            />
          </el-col>
          <el-col :span="amountColSpan">
            <amount-unit
              :ref="`creditAmount${index}`"
              class="grid-content last-amount-grid"
              :type="'value'"
              :active="activeAmountUnit"
              :editable="!canEdit"
              @enter="enterCeditAmount($event, detail, index)"
              @tab="enterCeditAmount($event, detail, index, true)"
              @input="amountInput('credit', detail, index)"
              @top="amountTopPriceFocus('credit', $event, detail, index)"
              @bottom="amountBottomPriceFocus('credit', $event, detail, index)"
              @type-equal="onTypeEqual('credit', detail, index)"
              @type-blank="onTypeBlank('credit', detail, index)"
              @closeOther="closeOther"
              v-model="detail.creditAmount"
            />
          </el-col>
          <el-col :span="1">
            <div class="grid-content delete-row-button">
              <i
                class="el-icon-circle-plus-outline"
                @click="copyRow(index)"
              ></i>
              <i class="icon-delete" @click="removeRow(index)"></i>
            </div>
          </el-col>
        </div>
      </div>
      <el-row class="voucher-body-last">
        <el-col :span="1">
          <div class="grid-content"></div>
        </el-col>
        <el-col :span="sumColSpan">
          <div class="grid-content">
            <div class="chinese-sum-amount">合计：{{ chineseSumAmount }}</div>
          </div>
        </el-col>
        <el-col :span="amountColSpan">
          <amount-unit
            class="grid-content"
            :type="'value'"
            :amount="voucherInfo.sumDebitAmount"
            :active="activeAmountUnit"
            @closeOther="closeOther"
            :editable="canEdit"
          />
        </el-col>
        <el-col :span="amountColSpan">
          <amount-unit
            class="grid-content last-amount-grid"
            :type="'value'"
            :amount="voucherInfo.sumCreditAmount"
            :active="activeAmountUnit"
            @closeOther="closeOther"
            :editable="canEdit"
          />
        </el-col>
        <el-col :span="1">
          <div class="grid-content"></div>
        </el-col>
      </el-row>
    </div>
    <div class="voucher-footer" v-if="type !== 'template'">
      <el-row>
        <el-col :span="5">制单人：{{ documentMaker }}</el-col>
        <el-col :span="8">制单时间：{{ voucherInfo.createTime }}</el-col>
        <el-col :span="5">审核人：{{ voucherInfo.auditorName }}</el-col>
        <el-col :span="6">审核时间：{{ voucherInfo.auditTime }}</el-col>
      </el-row>
    </div>
    <add-subject
      ref="addSubjectCheck"
      @add-account-success="addAccountSuccess"
      class="addSubjectDialog"
      :from="'voucherInput'"
      :dialogVisible="showAddSubject"
      :title="'新增科目'"
      :ntype="'voucherInput'"
      @click-cancel-form="cancelAddSubject"
      v-if="showAddSubject"
    />
    <add-customer
      v-if="showAssitAdd === 'assist1'"
      :title="'新增客户'"
      type="create"
      @close-model="closeAddAssit"
      @submit="addAssitSuccess"
    />
    <add-supplier
      v-if="showAssitAdd === 'assist2'"
      :title="'新增供应商'"
      type="create"
      @close-model="closeAddAssit"
      @submit="addAssitSuccess"
    />
    <add-staff
      v-if="showAssitAdd === 'assist3'"
      :title="'新增员工'"
      type="create"
      @close-model="closeAddAssit"
      @submit="addAssitSuccess"
    />
    <add-department
      v-if="showAssitAdd === 'assist4'"
      :title="'新增部门'"
      type="create"
      @close-model="closeAddAssit"
      @submit="addAssitSuccess"
    />
    <add-project
      v-if="showAssitAdd === 'assist5'"
      :title="'新增项目'"
      type="create"
      @close-model="closeAddAssit"
      @submit="addAssitSuccess"
    />
    <!-- <add-stock
      v-if="showAssitAdd === 'assist6'"
      :title="'新增存货及服务'"
      type="create"
      :ntype="'voucherInput'"
      @close-model="closeAddAssit"
      @submit="addAssitSuccess"
    /> -->
    <product-modal
      v-if="showAssitAdd === 'assist6'"
      :title="'新增存货及服务'"
      type="create"
      @submit="addAssitSuccess"
      @close-model="closeAddAssit"
    />
  </div>
</template>

<script>
import AmountName from "./Voucher/AmountName";
import AmountUnit from "./Voucher/AmountUnit";
import DigestSelect from "./Voucher/DigestSelect";
import AccountSelect from "./Voucher/AccountSelect";
import AssistSelect from "./Voucher/AssistSelect";
import accountBook from "@/api/accountBook/accountBook"; // 期间接口
import { mapState, mapGetters } from "vuex";
import bigdecimal from "bigdecimal";
import AmountUtil from "@/utils/AmountUtil";
import EventMixin from "@/mixin/event";
import { convertCurrency, getLastDay } from "@/tools/tools";
import VoucherInputAjax from "@/api/VoucherInput/VoucherInput";
import addSubject from "@/views/EnterpriseCenter/Subject/subpage/addSubject";
import addCustomer from "@/views/EnterpriseCenter/Customer/children/addEditModel"; // 客户
import addDepartment from "@/views/EnterpriseCenter/Department/children/addEditModel"; // 部门
import addProject from "@/views/EnterpriseCenter/project/children/addEditModel"; // 项目
import addStaff from "@/views/EnterpriseCenter/Staff/children/addEditModel"; // 员工
// import addStock from '@/views/EnterpriseCenter/Stock/children/addEditModel' // 存货
import productModal from "@/views/EnterpriseCenter/CommodityType/children/addEditModel"; // 新增存货及费用
import addSupplier from "@/views/EnterpriseCenter/Supplier/children/addEditModel"; // 供应商
import accountUploadBounced from "@/views/AccountSet/Voucher/AccountUploadBounced"; // 附件上传弹框

export default {
  name: "Voucher",
  mixins: [EventMixin],
  created() {
    this.saveType = this.type;
    this.initVoucherInfo();
    // 初始化四行分录
    this.initEmptyRows();
    this.$store.dispatch("common/getDigests");
    this.$store.dispatch("common/getLeafAccounts");
    this.$store.dispatch("common/getAssistCategories");
    if (this.$route.name === "voucherInput" && this.view === "input") {
      this.initEvent();
      this.getPeriod();
    } else if (this.$route.name === "voucherInfo" && this.view === "detail") {
      this.initEvent();
    }
    if (
      this.accountSet !== null &&
      this.type !== "template" &&
      this.type !== "insert"
    ) {
      this.initAccountSet(this.accountSet);
    }
    if (
      this.view === "input" &&
      this.voucherType === 0 &&
      (this.type === "add" || this.type === "insert")
    ) {
      this.toDigest(0);
    }
  },
  props: {
    type: {
      type: String,
      default: "add" // add, insert, edit, template
    },
    view: {
      type: String,
      default: "input" // 视图类型，input：凭证录入，detail：凭证详情
    },
    pageIsActive: {
      type: Boolean,
      default: true
    },
    voucherType: {
      type: Number,
      default: 0 // 凭证录入方式，0：手工，其他方式参考api中的说明
    },
    id: {
      type: Number,
      default: 0
    },
    insertParams: {
      type: Object,
      default: null
    },
    number: {
      type: Number,
      default: 0
    },
    voucher: {
      type: Object,
      default: null
    },
    fundId: {
      type: String,
      default: ""
    },
    voucherAddWay: {
      type: String,
      default: ""
    },
    source: {
      type: String,
      default: ''
    },
    voidFlag: {
      type: Boolean,
      default: true, // true 编辑状未作废  false 不能编辑状态作废
    }
  },
  data() {
    return {
      voucherInfo: null,
      voucherDetail: [],
      lastDigest: null,
      isShift: false,
      isTab: false,
      isMeta: false,
      saving: false,
      initVoucherData: null,
      saveType: "add",
      currentRowIndex: null,
      currentAccountRowIndex: null,
      currentAssistRowIndex: null,
      pickerOptions: {
        disabledDate: time => {
          return time.getTime() < this.currentFirstDayTime - 86400000;
        }
      },
      showAddSubject: false,
      showAssitAdd: null,
      // initLoadding: false,
      loadding: false,
      sourceRows: [],
      timers: {},
      isShowUploadBounced: false,
      upladFile: [],
      uploadFiles: [],
      activeAmountUnit: null
    };
  },
  methods: {
    closeOther(v) {
      this.activeAmountUnit = v;
    },
    getPeriod() {
      // 期间
      accountBook.QueryAccountPeriod().then(res => {
        if (res.code === 200) {
          const input = res.data.currentPeriod;
          const arr = input.split("-");
          const maxDay = new Date(arr[0], arr[1], 0).getDate();
          let date = arr[0] + arr[1];
          this.voucherInfo.date = arr[0] + "-" + arr[1] + "-" + maxDay;
          console.log(1111);
          this.generaterVoucherNumber(date, true);
        }
      });
    },
    // 借贷金额上下键切换行回调
    keyCodeCallBack() {},
    setUploadFiles(data, row) {
      this.uploadFiles = data;
      this.$refs.accountUploadBounced.setUploadFiles(data, row);
    },
    getUploadFile(files) {
      this.voucherInfo.attachmentCount = files.length;
      this.upladFile = files;
    },
    uploadBouncedIsShow() {
      this.isShowUploadBounced = !this.isShowUploadBounced;
    },
    upladRefresh(num) {},
    closeUploadBounced() {
      this.isShowUploadBounced = false;
    },
    getDefaultVoucherDetail() {
      return {
        digest: null,
        balance: "0",
        balanceSource: null,
        account: null,
        assist: null,
        isAssist: false,
        isQuantity: false,
        qty: "",
        price: "",
        debitAmount: "",
        creditAmount: "",
        // sourceDebitAmount: '',
        // sourceCreditAmount: '',
        // sourceAccountId: null,
        isEmpty: true, // 是否为空行
        active: false, // 是否当前活动行
        showAccount: true, // 是否显示科目
        showAssist: false, // 是否显示辅助核算
        balanceLoading: false
      };
    },
    loadBalance(detail, index) {
      if (detail.account !== null) {
        var timer = this.timers["time" + detail.account.id];
        if (timer) {
          clearTimeout(timer);
        }
        let periodArr = this.voucherInfo.date.split('-');
        let period = periodArr[0] + '' + periodArr[1]
        this.timers["time" + detail.account.id] = setTimeout(() => {
          VoucherInputAjax.QueryAccountBalancePeriod({
            accountId: detail.account.id, period
          }).then(response => {
            if (response.code === 200) {
              let { data } = response;
              if (data.direction === 1) {
                // 借
                // balanceSource = data.finalPeriodDebitAmount
                detail.balanceSource = data.finalPeriodDebitAmount;
              } else {
                // 贷
                // balanceSource = data.finalPeriodCreditAmount
                detail.balanceSource = data.finalPeriodCreditAmount;
              }
              // detail.balanceSource = balanceSource
              this.calcAllBalance();
            }
          });
        }, 400);
      } else {
        this.calcAllBalance();
      }
      // let balanceSource = null
      // // 先从现有科目中找到原始余额，如果找到就不必去接口中调取了
      // this.voucherDetail.forEach((item, i) => {
      //   if (item.account !== null && i !== index && detail.account != null) {
      //     if (item.account.id === detail.account.id) {
      //       balanceSource = item.balanceSource
      //       detail.balanceSource = balanceSource
      //     }
      //   }
      // })
      // // 没有找到，需要到接口中调取
      // if (balanceSource === null && detail.account !== null) {
    },
    closeModal() {
      // 现金费用页面点击金额时关闭弹窗
      this.$emit("close");
    },
    calcAllBalance() {
      this.voucherDetail.forEach((item, i) => {
        this.calcBalance(item, i);
      });
    },
    calcBalance(detail, index) {
      if (detail.account !== null && detail.balanceSource !== null) {
        let balance = this.getBigDecimal(detail.balanceSource);
        this.sourceRows.forEach(source => {
          if (source.sourceAccountId === detail.account.id) {
            // sameAccounts.push(item)
            if (detail.account.direction === 0) {
              balance = balance
                .add(this.getBigDecimal(source.sourceDebitAmount))
                .subtract(this.getBigDecimal(source.sourceCreditAmount));
            } else {
              balance = balance
                .subtract(this.getBigDecimal(source.sourceDebitAmount))
                .add(this.getBigDecimal(source.sourceCreditAmount));
            }
          }
        });
        let sameAccounts = [];
        // 这里计算后的结果为最终结果
        this.voucherDetail.forEach((item, i) => {
          if (item.account !== null && item.account.id === detail.account.id) {
            sameAccounts.push(item);
            if (detail.account.direction === 1) {
              balance = balance
                .add(this.getBigDecimal(item.debitAmount))
                .subtract(this.getBigDecimal(item.creditAmount));
            } else {
              balance = balance
                .subtract(this.getBigDecimal(item.debitAmount))
                .add(this.getBigDecimal(item.creditAmount));
            }
          }
        });
        // this.voucherDetail.forEach((item, i) => {
        //   // 科目id相同，但不是相同行的情况下进行累加计算
        //   if (item.account !== null && item.account.id === detail.account.id) {
        //     caclIds.push()
        //     sameAccounts.push(item)
        //     // （反向计算）排除原本凭证本身的原始金额（已经保存过的凭证），防止重复累加
        //     // if (detail.account.direction === 0) {
        //     //   balance = balance.add(this.getBigDecimal(item.sourceDebitAmount)).subtract(this.getBigDecimal(item.sourceCreditAmount))
        //     // } else {
        //     //   balance = balance.subtract(this.getBigDecimal(item.sourceDebitAmount)).add(this.getBigDecimal(item.sourceCreditAmount))
        //     // }
        //     // 计算原本有值
        //   }
        // })
        sameAccounts.forEach(item => {
          item.balance = balance.toString();
        });
      }
    },
    initEmptyRows() {
      this.voucherDetail = [];
      for (let i = 0; i < 4; i++) {
        this.voucherDetail.push(this.getDefaultVoucherDetail());
      }
    },
    initVoucherInfo() {
      this.voucherInfo = {
        id: 0,
        number: 1,
        status: 0,
        attachmentCount: 0,
        year: "",
        month: "",
        day: "",
        date: "",
        addWay: null,
        documentMaker: "",
        createTime: "",
        auditorName: "",
        auditTime: "",
        sumDebitAmount: "",
        sumCreditAmount: ""
      };
    },
    mouseoverRow(detail, index) {
      this.voucherDetail.forEach(item => {
        item.active = false;
      });
      detail.active = true;
      // 实时计算余额，请求较多
      clearTimeout(this.balanceLoading);
      this.balanceLoading = setTimeout(() => {
        this.loadBalance(detail, index);
      }, 200);
    },
    mouseoutRow(detail) {
      detail.active = false;
    },
    addAccount(index) {
      this.currentAccountRowIndex = index;
      this.showAddSubject = true;
      this.$nextTick(() => {
        this.$refs["account" + index][0].close();
        this.$refs.addSubjectCheck.$refs.form.$el[0].focus();
      });
    },
    addAccountSuccess(data) {
      this.toAmount(this.currentAccountRowIndex, true);
      this.$store.dispatch("common/getLeafAccounts").then(() => {
        this.showAddSubject = false;
        let detail = this.voucherDetail[this.currentAccountRowIndex];
        this.changeAccount(data, detail, this.currentAccountRowIndex);
        this.enterAccount(data, detail, this.currentAccountRowIndex);
        // 没有辅助核算也没有数量核算
      });
    },
    addAssist(index, detail) {
      if (detail.account === null) {
        return;
      }
      this.currentAssistRowIndex = index;
      let category = this.assistCategories.find(item => {
        return item.id === detail.account.assistCategoryId;
      });
      this.$refs["assist" + index][0].close();
      this.showAssitAdd = "assist" + category.type;
    },
    closeAddAssit() {
      this.showAssitAdd = null;
    },
    addAssitSuccess(status, data) {
      this.showAssitAdd = null;
      let detail = this.voucherDetail[this.currentAssistRowIndex];
      this.$store
        .dispatch("common/getAssists", detail.account.assistCategoryId)
        .then(() => {
          this.changeAssist(data.assist, detail, this.currentAssistRowIndex);
          this.enterAssist(data.assist, detail, this.currentAssistRowIndex);
        });
    },
    cancelAddSubject() {
      this.showAddSubject = false;
    },
    accountQtyInput(value, detail, index) {
      detail.qty = AmountUtil.parseNumber(detail.qty, false, true, 8, 12, 2);
      this.caclAmountByPriceQty(detail, index);
      let type = "credit";
      if (detail.creditAmount !== "") {
        type = "debit";
      }
      this.caclPriceQtyByAmount(type, detail, index);
    },
    accountPriceInput(value, detail, index) {
      detail.price = AmountUtil.parseNumber(
        detail.price,
        false,
        false,
        8,
        12,
        2
      ).replace("-", "");
      this.caclAmountByPriceQty(detail, index);
    },
    accountQtyFocus(event, detail, index) {
      this.$nextTick(() => {
        this.$refs[`accountQty${index}`][0].select();
      });
    },
    // 金额上键
    amountTopPriceFocus(val, event, detail, index) {
      this.$nextTick(() => {
        if (val == "credit") {
          this.$refs[`creditAmount${index - 1}`][0].focus();
        } else {
          this.$refs[`debitAmount${index - 1}`][0].focus();
        }
      });
    },
    // 金额下键
    amountBottomPriceFocus(val, event, detail, index) {
      this.$nextTick(() => {
        if (val == "credit") {
          this.$refs[`creditAmount${index + 1}`][0].focus();
        } else {
          this.$refs[`debitAmount${index + 1}`][0].focus();
        }
      });
    },
    accountPriceFocus(event, detail, index) {
      this.$nextTick(() => {
        this.$refs[`accountPrice${index}`][0].select();
      });
    },
    amountInput(type, detail, index) {
      // 处理输入金额事件
      if (type === "credit") {
        if (detail.creditAmount !== "") {
          detail.debitAmount = "";
        }
      } else {
        if (detail.debitAmount !== "") {
          detail.creditAmount = "";
        }
      }
      // if (detail.debitAmount !== '' || detail.creditAmount !== '') {
      this.caclPriceQtyByAmount(type, detail, index);
      // }
      // this.calcBalance(detail, index)
      // this.sumAmount(this.voucherDetail)
    },
    onTypeBlank(type, detail, index) {
      let cds = { credit: "debit", debit: "credit" };
      // 处理输入空格事件
      this.$nextTick(() => {
        if (type === "credit") {
          this.toDebitAmount(index);
          if (detail.creditAmount !== "") {
            detail.debitAmount = detail.creditAmount;
            detail.creditAmount = "";
          }
        } else {
          this.toCreditAmount(index);
          if (detail.debitAmount !== "") {
            detail.creditAmount = detail.debitAmount;
            detail.debitAmount = "";
          }
        }
        this.caclPriceQtyByAmount(cds[type], detail, index);
        this.sumAmount(this.voucherDetail);
      });
    },
    onTypeEqual(type, detail, index) {
      // 处理输入等号事件
      // 首先要把对象方向的值清除
      if (type === "credit") {
        detail.debitAmount = "";
      } else {
        detail.creditAmount = "";
      }
      // 清除对方金额完成后，需要重新计算合计金额
      this.caclEqual(type, detail, index);
      this.caclPriceQtyByAmount(type, detail, index);
      this.sumAmount(this.voucherDetail);
    },
    stringToDigestObject(value) {
      return { id: value, name: value };
    },
    changeDigest(value, detail, index) {
      if (typeof value === "string") {
        value = this.stringToDigestObject(value);
      }
      detail.digest = value;
      this.lastDigest = detail.digest;
      this.$store.dispatch("common/addTempDigest", value);
    },
    handleDeleteOption(option) {
      VoucherInputAjax.deleteDigestOption({ id: option.id }).then(res => {
        if (res.code === 200) {
          this.$store.dispatch("common/getDigests");
        }
      });
    },
    changeAccount(value, detail, index) {
      if (value === null) {
        return;
      }
      detail.account = value;
      if (
        detail.assist !== null &&
        detail.assist.categoryId !== value.assistCategoryId
      ) {
        detail.assist = null;
      }
      detail.isQuantity = value.isQuantity;
      detail.isAssist = value.isAssist;
      // 调用余额，并实时计算余额
      this.loadBalance(detail, index);
    },
    changeAssist(value, detail, index) {
      if (value === null) {
        return;
      }
      detail.assist = value;
    },
    accountVisibleChange(isShow, detail, index) {
      // 判断科目是辅助核算类科目，但没选择辅助核算的情况
      if (
        !isShow &&
        detail.account &&
        detail.account.isAssist &&
        detail.assist === null &&
        detail.account.showAccount
      ) {
        this.toAssist(index);
      }
      if (isShow) {
        this.currentRowIndex = index;
      } else {
        this.currentRowIndex = null;
      }
    },
    assistVisibleChange(isShow, detail, index) {
      // 判断隐藏了辅助核算，但什么都没选的情况
      if (
        !isShow &&
        this.showAssitAdd === null &&
        detail.account &&
        detail.isAssist &&
        detail.assist === null &&
        detail.account.showAccount
      ) {
        this.toAssist(index);
      } else if (!isShow) {
        detail.showAssist = false;
        detail.showAccount = true;
        document.body.style.overflow = "hidden";
      }
    },
    // 准备离开摘要
    enterDigest(value, detail, index, isTab = false, isConfirm = true) {
      this.$refs[`digest${index}`][0].close();
      if (typeof value === "string") {
        value = this.stringToDigestObject(value);
      }
      detail.digest = value;
      this.lastDigest = detail.digest;
      if (isTab && this.isShift) {
        // tab+shift，到本行金额
        this.toAmount(index, isTab);
      } else if (!isTab && this.isShift) {
        // 回车+shift，需要到上一行
        if (index > 0) {
          this.toAmountOrPreLine(detail, index, isTab);
        } else {
          this.toDigest(index, isTab);
        }
      } else if (isConfirm) {
        // 转到本行科目录入
        this.toAccount(index);
      }
    },
    // 准备离开科目
    enterAccount(value, detail, index, isTab = false) {
      this.$refs[`account${index}`][0].close();
      this.changeAccount(value, detail, index);
      if (this.isShift) {
        // (tab|enter) + shift，到本行摘要
        this.toDigest(index, isTab);
      } else {
        // 本行下一个, 当tab键的时候，就不管有没有辅助核算了
        if (value && value.isAssist && (!isTab || !detail.assist)) {
          // 辅助核算时
          this.toAssist(index);
        } else if (value && value.isQuantity) {
          // 数量核算时跳转到数量
          this.toAccountQty(index);
        } else {
          // 没有辅助核算也没有数量核算
          this.toAmount(index, isTab);
        }
      }
    },
    // 准备离开辅助核算
    enterAssist(value, detail, index, isTab = false) {
      detail.showAccount = true;
      detail.showAssist = false;
      document.body.style.overflow = "hidden";
      if (value === null) {
        // 如果没有选择辅助核算，重新打开科目选择
        this.toAccount(index);
        return;
      }
      if (this.isShift) {
        // (tab|enter) + shift，到本行摘要
        this.toDigest(index, isTab);
      } else {
        // 本行下一个
        if (detail.isQuantity) {
          // 数量核算时跳转到数量
          this.toAccountQty(index);
        } else {
          // 没有辅助核算也没有数量核算
          this.toAmount(index, isTab);
        }
      }
    },
    // 准备离开数量核算数量
    enterAccountQty(event, detail, index, isTab = false) {
      this.stopDefaultEvent(event);
      this.blurAccountQty(event.target.value, detail, index, isTab);
      if (this.isShift) {
        // 转到科目录入
        this.toAccount(index);
      } else {
        this.toAccountPrice(index);
      }
    },
    // blur数量核算数量
    blurAccountQty(value, detail, index, isTab = false) {
      detail.qty = AmountUtil.parseNumber(detail.qty, true, true, 8, 12, 2);
    },
    // 准备离开数量核算单价
    enterAccountPrice(event, detail, index, isTab = false) {
      this.stopDefaultEvent(event);
      this.blurAccountPrice(event.target.value, detail, index, isTab);
      if (this.isShift) {
        // 转到数量录入
        this.toAccountQty(index);
      } else {
        this.toAmount(index, isTab);
      }
    },
    // blur数量核算单价
    blurAccountPrice(value, detail, index, isTab = false) {
      detail.price = AmountUtil.parseNumber(
        detail.price,
        true,
        false,
        8,
        12,
        2
      );
    },
    // 准备离开金额
    enterBerKeyEvent(event, detail, index, isTab = false) {
      if (event.keyCode === 38) {
        // 上
      } else if (event.keyCode === 40) {
        // 下
      }
    },
    // 准备离开借方金额
    enterDebitAmount($event, detail, index, isTab = false) {
      if (this.isShift) {
        this.mabyToAccountOrPrice(index);
      } else {
        // this.toCreditAmount(index)
        if (
          // !this.isEmpty(detail.creditAmount) || // for bug6938
          !this.isEmpty(detail.debitAmount)
        ) {
          this.toDigestOrNextLine(detail, index, isTab);
        } else {
          // 跳到贷方金额
          this.toCreditAmount(index);
        }
      }
    },
    // 准备离开贷方金额
    enterCeditAmount($event, detail, index, isTab = false) {
      if (this.isShift) {
        if (this.isEmpty(detail.creditAmount)) {
          this.toDebitAmount(index);
        } else {
          this.mabyToAccountOrPrice(index);
        }
      } else {
        if (
          !this.isEmpty(detail.creditAmount) ||
          !this.isEmpty(detail.debitAmount)
        ) {
          this.toDigestOrNextLine(detail, index, isTab);
        } else {
          // 跳到摘要
          this.toDigest(index, isTab);
        }
      }
    },
    toNextLine(detail, index, isTab = false) {
      let i = index + 1;
      if (!this.voucherDetail[i]) {
        this.voucherDetail.push(this.getDefaultVoucherDetail());
      }
      this.toDigest(i, isTab, true);
    },
    toDigestOrNextLine(detail, index, isTab = false) {
      if (isTab || detail.digest === null || detail.account === null) {
        // 跳到摘要
        this.toDigest(index, isTab);
      } else {
        // 跳到下一行的摘要
        this.toNextLine(detail, index, isTab);
      }
    },
    toAmountOrPreLine(detail, index, isTab = false) {
      if (
        index > 0 &&
        !isTab &&
        this.isShift &&
        detail.digest !== null &&
        detail.account !== null &&
        (detail.debitAmount !== "" || detail.creditAmount !== "")
      ) {
        // 跳到上一行
        index--;
        this.toAmount(index, isTab);
      } else {
        // 跳到本行金额
        this.toAmount(index, isTab);
      }
    },
    toAmount(index, isTab = false) {
      // if (this.isShift) {
      //   this.toCreditAmount(index)
      // } else {
      //   this.toDebitAmount(index)
      // }
      if (this.voucherDetail[index]) {
        let detail = this.voucherDetail[index];
        if (this.isShift) {
          // 如果两个都没有金额，并且按了shift应该从贷方金额开始
          if (detail.debitAmount !== "") {
            this.toDebitAmount(index);
          } else {
            this.toCreditAmount(index);
          }
        } else {
          // 以下注释掉的作用是为了实现bug6938需求
          this.toDebitAmount(index);
          // if (detail.creditAmount !== '') {
          //   this.toCreditAmount(index)
          // } else {
          //   this.toDebitAmount(index)
          // }
        }
      }
    },
    toDebitAmount(index) {
      this.$refs[`debitAmount${index}`][0].onValueClick();
    },
    toCreditAmount(index) {
      this.$refs[`creditAmount${index}`][0].onValueClick();
    },
    toAccount(index) {
      let detail = this.voucherDetail[index];
      detail.showAccount = true;
      detail.showAssist = false;
      document.body.style.overflow = "hidden";
      this.$nextTick(() => {
        this.$refs[`account${index}`][0].focus();
      });
    },
    toAssist(index) {
      let detail = this.voucherDetail[index];
      detail.showAccount = false;
      detail.showAssist = true;
      document.body.style.overflow = "auto";
      this.$nextTick(() => {
        this.$refs[`assist${index}`][0].focus();
      });
    },
    toDigest(index, isTab = false, isNewLine = false) {
      this.$nextTick(() => {
        this.$refs[`digest${index}`][0].focus();
        let detail = this.voucherDetail[index];
        if (
          detail.debitAmount === "" &&
          detail.creditAmount === "" &&
          isNewLine
        ) {
          this.caclEqual("", detail, index);
        }
      });
    },
    toAccountQty(index) {
      this.$nextTick(() => {
        this.$refs[`accountQty${index}`][0].focus();
      });
    },
    toAccountPrice(index) {
      this.$nextTick(() => {
        this.$refs[`accountPrice${index}`][0].focus();
      });
    },
    mabyToAccountOrPrice(index) {
      if (this.voucherDetail[index]) {
        let detail = this.voucherDetail[index];
        if (detail.isQuantity) {
          // 数量核算时跳转到数量
          this.toAccountPrice(index);
        } else {
          // 没有数量核算,转到科目
          this.toAccount(index);
        }
      }
    },
    addRowToBottom(index) {
      index = index + 1;
      this.addRowToIndex(index);
    },
    addRowToIndex(index) {
      let newDetail = this.getDefaultVoucherDetail();
      this.voucherDetail.splice(index, 0, newDetail);
    },
    removeRow(index, val, saveAndNew) {
      this.voucherDetail.splice(index, 1);
      if (this.voucherDetail.length < 4) {
        this.addRowToIndex(this.voucherDetail.length);
      }
      this.sumAmount(this.voucherDetail);
      this.calcAllBalance();
      if (val == "save") {
        if (saveAndNew) {
          this.save(true);
        } else {
          this.save();
        }
      }
    },
    // 复制行
    copyRow(index) {
      this.voucherDetail.splice(
        index,
        0,
        JSON.parse(JSON.stringify(this.voucherDetail[index]))
      );
      if (this.voucherDetail.length < 4) {
        this.addRowToIndex(this.voucherDetail.length);
      }
      this.sumAmount(this.voucherDetail);
      this.calcAllBalance();
    },
    caclAmountByPriceQty(detail, index) {
      if (detail.qty !== "" && detail.price !== "") {
        let qty = this.getBigDecimal(detail.qty);
        let price = this.getBigDecimal(detail.price);
        let amount = qty.multiply(price);
        let amountString = AmountUtil.parseNumber(amount.toString(), true);
        if (detail.creditAmount !== "") {
          detail.creditAmount = amountString;
        } else {
          detail.debitAmount = amountString;
        }
        this.calcBalance(detail, index);
        this.sumAmount(this.voucherDetail);
      }
    },
    caclPriceQtyByAmount(type, detail, index) {
      if (detail.isQuantity) {
        let amountValue =
          detail.creditAmount !== "" ? detail.creditAmount : detail.debitAmount;
        if (amountValue !== "") {
          let amount = this.getBigDecimal(amountValue);
          // 负数情况不能设置单价
          let priceString = "";
          if (detail.qty !== "" && amount.toString().indexOf("-") === -1) {
            let qty = this.getBigDecimal(detail.qty);
            let price = amount.divide(
              qty,
              8,
              bigdecimal.RoundingMode.HALF_UP()
            );
            priceString = AmountUtil.parseNumber(
              price.toString(),
              true,
              false,
              8,
              12,
              2
            );
          }
          if (priceString === "" || priceString.indexOf("-") !== -1) {
            let price = this.getBigDecimal(detail.price);
            let qty = amount.divide(
              price,
              8,
              bigdecimal.RoundingMode.HALF_UP()
            );
            let qtyString = AmountUtil.parseNumber(
              qty.toString(),
              true,
              false,
              8,
              12,
              2
            );
            detail.qty = qtyString;
          } else {
            detail.price = priceString;
          }
        } else {
          detail.price = "";
        }
      }
      this.sumAmount(this.voucherDetail);
    },
    caclEqual(type, detail, index) {
      // 清除对方金额完成后，需要重新计算合计金额
      let sumCreditAmount = this.getBigDecimal(
        this.voucherInfo.sumCreditAmount
      );
      let sumDebitAmount = this.getBigDecimal(this.voucherInfo.sumDebitAmount);
      // 因为金额合计里面包含了借贷方向的金额，所以这里需要把原本输入框内的值累加上去才能相等
      let creditAmount = this.getBigDecimal(detail.creditAmount);
      let debitAmount = this.getBigDecimal(detail.debitAmount);
      let differ1 = sumDebitAmount.subtract(sumCreditAmount).add(creditAmount);
      let differ2 = sumCreditAmount.subtract(sumDebitAmount).add(debitAmount);
      if (type === "") {
        // 自动判断应该放到哪个方向
        if (differ1.toString().indexOf("-") === -1) {
          // 判断是否包含负号
          detail.creditAmount =
            differ1.toString() === "0" ? "" : differ1.toString();
        } else {
          detail.debitAmount =
            differ2.toString() === "0" ? "" : differ2.toString();
        }
      } else if (type === "credit") {
        detail.creditAmount =
          differ1.toString() === "0" ? "" : differ1.toString();
      } else {
        detail.debitAmount =
          differ2.toString() === "0" ? "" : differ2.toString();
      }
      this.sumAmount(this.voucherDetail);
    },
    sumAmount(voucherDetailList) {
      // 计算合计金额
      let sumCreditAmount = this.getBigDecimal("0");
      let sumDebitAmount = this.getBigDecimal("0");
      voucherDetailList.forEach((detail, index) => {
        sumCreditAmount = this.getBigDecimal(detail.creditAmount).add(
          sumCreditAmount
        );
        sumDebitAmount = this.getBigDecimal(detail.debitAmount).add(
          sumDebitAmount
        );
      });
      if (sumCreditAmount !== null) {
        this.voucherInfo.sumCreditAmount = AmountUtil.parseNumber(
          sumCreditAmount.toString(),
          true,
          false
        );
      }
      if (sumDebitAmount !== null) {
        this.voucherInfo.sumDebitAmount = AmountUtil.parseNumber(
          sumDebitAmount.toString(),
          true,
          false
        );
      }
    },
    getBigDecimal(value) {
      let val = value.toString();
      if (val === "" || val === "-" || val === ".") {
        val = "0";
      }
      return new bigdecimal.BigDecimal(val);
    },
    isEmpty(value) {
      return (
        value === null ||
        value === "" ||
        value === "0" ||
        parseFloat(value) === 0
      );
    },
    initKeyUpEvent(event) {
      this.isShift = event.shiftKey;
      this.isCtrl = event.ctrlKey;
      this.isMeta = event.key === "Meta" ? false : this.isMeta;
    },
    initKeyDownEvent(event) {
      this.isShift = event.shiftKey;
      this.isCtrl = event.ctrlKey;
      this.isMeta = this.isMeta || event.key === "Meta";
      if (
        (event.keyCode === 83 || event.which === 83) &&
        (event.ctrlKey || this.isMeta)
      ) {
        this.stopDefaultEvent(event);
        if (this.pageIsActive) {
          this.save();
        }
      } else if (
        event.keyCode === 123 ||
        event.which === 123 ||
        event.keyCode === 176 ||
        event.which === 176
      ) {
        this.stopDefaultEvent(event);
        if (this.pageIsActive) {
          this.save(true);
        }
      } else if (
        event.keyCode === 121 ||
        event.which === 121 ||
        event.keyCode === 177 ||
        event.which === 177
      ) {
        this.stopDefaultEvent(event);
        if (this.pageIsActive && this.currentRowIndex !== null) {
          this.addAccount(this.currentRowIndex);
        }
      }
    },
    save(saveAndNew = false) {
      if (this.saving) {
        return;
      }
      if (!this.validateSaveData(saveAndNew)) {
        return;
      }
      this.saving = true;
      let postData = this.packageData();
      if (this.upladFile.length) {
        postData.voucherAttachmentList = this.upladFile;
      }
      if (this.saveType === "add") {
        if (this.voucherAddWay == "2") {
          postData.fundId = this.fundId;
        }
        //保存凭证记录当前凭证日期
        sessionStorage.setItem(
          "voucherInfo_date" + sessionStorage.getItem("customerId"),
          postData.date
        );
        VoucherInputAjax.AddVoucher(postData).then(response => {
          this.afterSave(response, saveAndNew);
        });
      } else if (this.saveType === "insert") {
        postData.afterVoucherId = this.insertParams.voucherId;
        VoucherInputAjax.InsertVoucher(postData).then(response => {
          this.afterSave(response, saveAndNew);
        });
      } else if (this.saveType === "edit") {
        postData.id = this.voucherInfo.id;
        VoucherInputAjax.UpdateVoucher(postData).then(response => {
          this.afterSave(response, saveAndNew);
        });
      }
    },
    changeVoucherNumber(type) {
      if (type === "add") {
        this.voucherInfo.number++;
      } else {
        this.voucherInfo.number--;
      }
      this.validateVoucherNumber();
    },
    numberKeyEvent(event) {
      this.stopDefaultEvent(event);
      if (event.keyCode === 38) {
        // 上
        this.changeVoucherNumber("add");
      } else if (event.keyCode === 40) {
        // 下
        this.changeVoucherNumber("reduce");
      }
    },
    inputVoucherNumber(value) {
      let number = parseInt(value.target.value.replace(/[^\d]/g, ""));
      if (isNaN(number)) {
        number = "";
      }
      this.voucherInfo.number = number;
      this.validateVoucherNumber();
    },
    validateVoucherNumber() {
      if (this.voucherInfo.number === "") {
        return;
      }
      if (this.voucherInfo.number < 1) {
        this.voucherInfo.number = 1;
      } else if (this.voucherInfo.number > 9999) {
        this.voucherInfo.number = 9999;
      }
    },
    inputAttachmentCount(value) {
      let number = parseInt(value.target.value.replace(/[^\d]/g, ""));
      if (isNaN(number)) {
        number = "";
      }
      this.voucherInfo.attachmentCount = number;
      this.validateAttachmentCount();
    },
    attachmentCountKeyEvent(event) {
      this.stopDefaultEvent(event);
      if (event.keyCode === 38) {
        // 上
        this.changeAttachmentCount("add");
      } else if (event.keyCode === 40) {
        // 下
        this.changeAttachmentCount("reduce");
      }
    },
    changeAttachmentCount(type) {
      if (type === "add") {
        this.voucherInfo.attachmentCount++;
      } else {
        this.voucherInfo.attachmentCount--;
      }
      this.validateAttachmentCount();
    },
    validateAttachmentCount() {
      if (this.voucherInfo.attachmentCount === "") {
        return;
      }
      if (this.voucherInfo.attachmentCount < 0) {
        this.voucherInfo.attachmentCount = 0;
      } else if (this.voucherInfo.attachmentCount > 9999) {
        this.voucherInfo.attachmentCount = 9999;
      }
    },
    attachmentCountBlur(event) {
      let value = parseInt(event.target.value);
      this.voucherInfo.attachmentCount = isNaN(value) ? 0 : value;
    },
    generaterVoucherNumber(currentPeriod, changeDate = false) {
      console.log('0000');
      currentPeriod =
        this.source == "settleAccounts"
          ? this.voucherInfo.date.replaceAll("-", "").slice(0, 6)
          : currentPeriod;
      let query = { period: currentPeriod };
      // if (this.loadding) return
      // this.loadding = true
      VoucherInputAjax.GeneraterVoucherNumber({ ...query }).then(response => {
        this.loadding = false;
        if (response.code === 200) {
          this.voucherInfo.number =
            response.data >= 9999 ? 9999 : response.data;
          this.voucherInfo.year = currentPeriod.toString().slice(0, 4);
          this.voucherInfo.month = currentPeriod.toString().slice(4);
          if (
            (changeDate || !this.voucherInfo.date) &&
            this.$route.name !== "certification"
          ) {
            let lastDay = getLastDay(
              this.voucherInfo.year,
              this.voucherInfo.month
            );
            this.voucherInfo.date = `${this.voucherInfo.year}-${this.voucherInfo.month}-${lastDay}`;
            if (this.source == "settleAccounts") {
              if (sessionStorage.getItem("checkoutDate")) {
                const input = sessionStorage.getItem("checkoutDate");
                const arr = input.split("-");
                const maxDay = new Date(arr[0], arr[1], 0).getDate();
                this.voucherInfo.date = arr[0] + "-" + arr[1] + "-" + maxDay;
              } else {
                this.voucherInfo.date = this.voucherInfo.date;
              }
            } else {
              if (
                sessionStorage.getItem(
                  "voucherInfo_date" + sessionStorage.getItem("customerId")
                )
              ) {
                this.voucherInfo.date = sessionStorage.getItem(
                  "voucherInfo_date" + sessionStorage.getItem("customerId")
                );
              }
            }
          }
        }
      });
    },
    validateSaveData(saveAndNew) {
      let list = this.voucherDetail;
      let successCount = 0;
      for (var i = 0; i < list.length; i++) {
        let item = list[i];
        if (
          item.digest !== null ||
          item.account !== null ||
          item.debitAmount !== "" ||
          item.creditAmount !== ""
        ) {
          let inputCount = 0;
          if (item.digest !== null) {
            inputCount++;
          }
          if (item.account !== null) {
            inputCount++;
          }
          if (item.debitAmount !== "") {
            inputCount++;
          }
          if (item.creditAmount !== "") {
            inputCount++;
          }
          if (inputCount === 1) {
            this.removeRow(i, "save", saveAndNew);
            // this.$message({
            //   message: '请录入凭证信息！',
            //   type: 'error'
            // })
            return false;
          }
          if (item.digest === null) {
            //  if(item.debitAmount === '' && item.creditAmount === ''){

            // }else {
            //   this.$message({
            //     message: '摘要未录入！',
            //     type: 'error'
            //   })
            // }
            this.removeRow(i, "save", saveAndNew);
            return false;
          }
          if (item.account === null) {
            // if(item.debitAmount === '' && item.creditAmount === ''){
            //    this.removeRow (i,'save',saveAndNew)
            // }else {
            //   this.$message({
            //     message: '科目未录入！',
            //     type: 'error'
            //   })
            // }
            this.removeRow(i, "save", saveAndNew);
            return false;
          }
          if (item.isQuantity && (item.qty === "" || item.price === "")) {
            this.$message({
              message: "请录入数量与单价信息！",
              type: "error"
            });
            return false;
          }
          if (
            item.isQuantity &&
            this.view === "detail" &&
            (item.qty === "0" || item.price === "0")
          ) {
            this.$message({
              message: "请录入数量与单价信息！",
              type: "error"
            });
            return false;
          }
          if (
            item.isQuantity &&
            (item.qty === "" ||
              (item.debitAmount === "" && item.creditAmount === ""))
          ) {
            this.$message({
              message: "请录入数量与金额信息！",
              type: "error"
            });
            return false;
          }
          if (
            item.isQuantity &&
            (item.price === "" ||
              (item.debitAmount === "" && item.creditAmount === ""))
          ) {
            this.$message({
              message: "请录入金额与单价信息！",
              type: "error"
            });
            return false;
          }
          if (item.debitAmount === "" && item.creditAmount === "") {
            // this.$message({
            //   message: '请录入借贷方金额！',
            //   type: 'error'
            // })
            this.removeRow(i, "save", saveAndNew);
            return false;
          }
          if (
            this.voucherInfo.sumDebitAmount !== this.voucherInfo.sumCreditAmount
          ) {
            this.$message({
              message: "借贷不平衡，请检查后再保存！",
              type: "error"
            });
            return false;
          }
          successCount++;
        }
      }
      if (successCount <= 0) {
        this.$message({
          message: "请录入凭证信息！",
          type: "error"
        });
      }
      return successCount > 0;
    },
    packageData() {
      let postData = {
        attachmentCount: this.voucherInfo.attachmentCount,
        date: this.voucherInfo.date,
        number: this.voucherInfo.number,
        word: "记",
        voucherDetailFormList: [],
        addWay: this.voucherInfo.addWay
      };
      this.voucherDetail.forEach((item, index) => {
        if (
          item.digest !== null ||
          item.account !== null ||
          item.debitAmount !== "" ||
          item.creditAmount !== ""
        ) {
          let voucherItem = {
            accountId: item.account ? Number(item.account.id) : null,
            amount:
              Number(
                item.debitAmount !== "" ? item.debitAmount : item.creditAmount
              ) ||
              Number(
                item.creditAmount !== "" ? item.creditAmount : item.debitAmount
              ),
            amountDirection:
              Number(item.creditAmount) === 0 && Number(item.debitAmount) !== 0
                ? 1
                : 0,
            assistId: !item.assist ? null : Number(item.assist.id),
            digest: item.digest ? item.digest.name : "",
            rowNumber: index + 1,
            unit: "",
            voucherId: this.voucherInfo.id,
            id: item.id
          };
          if (item.isQuantity) {
            voucherItem.quantity = item.qty;
            voucherItem.unitPrice = item.price;
          } else {
            voucherItem.quantity = "";
          }
          postData.voucherDetailFormList.push(voucherItem);
        }
      });
      return postData;
    },
    afterSave(response, saveAndNew = false) {
      this.saving = false;
      if (response.code === 200) {
        // this.insertParams = null
        this.$store.dispatch("common/getDigests");
        let message = "凭证保存成功！";
        if (this.saveType === "insert") {
          message = "凭证插入成功！";
        }
        this.$message({
          message: message,
          type: "success"
        });
        if (this.view === "detail") {
          saveAndNew = false;
        }
        this.$emit("after-save", {
          success: true,
          saveAndNew: saveAndNew,
          voucher: response.data
        });
        if (saveAndNew && this.view === "input") {
          this.voucherDetail = [];
          this.initVoucherInfo();
          this.initEmptyRows();
          this.toDigest(0);
          this.$store.dispatch("common/getAccountSet");
          this.$refs.accountUploadBounced.clearFileArr();
        }
      } else {
        this.$message({
          message: response.code !== 400 ? "保存失败" : response.message,
          type: "error"
        });
        this.$emit("after-save", {
          success: false,
          saveAndNew: saveAndNew,
          voucher: response.data
        });
      }
    },
    clearFileArrFun() {
      this.$refs.accountUploadBounced.clearFileArr();
    },
    copyFun() {
      let voucherDetail = this.voucherDetail;
      let copyData = {
        customerId: sessionStorage.getItem("customerId"),
        voucherDetail: voucherDetail
      };
      sessionStorage.setItem("copyVoucherData", JSON.stringify(copyData));
      this.$message({
        message: "复制成功！",
        type: "success"
      });
    },
    pasteFun() {
      let copyVoucherData = JSON.parse(
        sessionStorage.getItem("copyVoucherData")
      );
      let voucherDetail = copyVoucherData.voucherDetail || [];
      let customerId = sessionStorage.getItem("customerId");
      if (customerId === copyVoucherData.customerId) {
        this.voucherDetail.splice(0, this.voucherDetail.length);
        this.voucherDetail = voucherDetail;
        voucherDetail.forEach((detail, index) => {
          detail.qty = detail.qty === "0" ? "" : detail.qty;
          this.caclAmountByPriceQty(detail, index);
          this.loadBalance(detail, index);
        });
        this.sumAmount(this.voucherDetail);
      }
    },
    useTemplate(template) {
      let items = template.voucherTemplateDetailList;
      this.voucherDetail = [];
      items.forEach((item, index) => {
        let detail = this.getDefaultVoucherDetail();
        detail.digest = { id: item.digest, name: item.digest };
        detail.account = item.account;
        detail.balance = "0";
        detail.assist = item.assist;
        detail.isAssist = item.isAssist;
        detail.isQuantity = item.isQuantity;
        detail.qty = (item.creditQuantity === 0
          ? item.debitQuantity
          : item.creditQuantity
        ).toString();
        detail.qty = detail.qty === "0" ? "" : detail.qty;
        detail.price = item.unitPrice === 0 ? "" : item.unitPrice.toString();
        this.voucherDetail.push(detail);
        // 保存模板 录入显示借贷金额
        // this.changeAccount(detail.account, detail, index)
        // if (item.direction === 1) {
        //   detail.debitAmount = '0'
        // } else {
        //   detail.creditAmount = '0'
        // }
        this.caclAmountByPriceQty(detail, index);
        detail.debitAmount =
          item.debitAmount === "0" ? "" : item.debitAmount.toString();
        detail.creditAmount =
          item.creditAmount === "0" ? "" : item.creditAmount.toString();
        this.loadBalance(detail, index);
      });
      for (let i = 0; i < 4 - items.length; i++) {
        this.voucherDetail.push(this.getDefaultVoucherDetail());
      }
      this.sumAmount(this.voucherDetail);
    },
    setVoucher (voucher) {
      // period 凭证期间
      let month = voucher.month < 10 ? "0" + voucher.month : voucher.month;
      let year = voucher.year;
      if (voucher !== null) {
        this.voucherDetail = [];
        this.sourceRows = [];
        for (var index = 0; index < voucher.voucherDetailList.length; index++) {
          var item = voucher.voucherDetailList[index];
          let detail = this.getDefaultVoucherDetail();
          detail.id = item.id;
          detail.digest = { id: item.digest, name: item.digest };
          detail.account = item.account;
          detail.balance = "0";
          detail.assist = item.assist;
          detail.isAssist = item.account.isAssist;
          detail.isQuantity = item.account.isQuantity;
          detail.qty = (item.creditQuantity === 0
            ? item.debitQuantity
            : item.creditQuantity
          ).toString();
          detail.price = item.unitPrice.toString();
          detail.debitAmount =
            item.debitAmount === 0 ? "" : item.debitAmount.toString();
          detail.creditAmount =
            item.creditAmount === 0 ? "" : item.creditAmount.toString();
          if (item.id) {
            this.sourceRows.push({
              sourceAccountId: detail.account.id,
              sourceDebitAmount: detail.debitAmount,
              sourceCreditAmount: detail.creditAmount
            });
          }
          this.voucherDetail.push(detail);
          // this.loadBalance(detail, index)
        }
        // 保存之后更新余额
        this.voucherDetail.forEach((item, index) => {
          this.loadBalance(item, index);
        });
        for (let i = 0; i < 4 - voucher.voucherDetailList.length; i++) {
          this.voucherDetail.push(this.getDefaultVoucherDetail());
        }
        this.sumAmount(voucher.voucherDetailList);
        this.voucherInfo.id = voucher.id;
        this.voucherInfo.number = voucher.number;
        this.voucherInfo.status = voucher.status;
        if (this.source == "settleAccounts") {
          if (sessionStorage.getItem("checkoutDate")) {
            // this.voucherInfo.date = sessionStorage.getItem("checkoutDate")
            const input = sessionStorage.getItem("checkoutDate");
            const arr = input.split("-");
            const maxDay = new Date(arr[0], arr[1], 0).getDate();
            this.voucherInfo.date = arr[0] + "-" + arr[1] + "-" + maxDay;
          } else {
            this.voucherInfo.date = voucher.date;
          }
        } else {
          this.voucherInfo.date = voucher.date;
        }

        this.voucherInfo.attachmentCount = voucher.attachmentCount;
        this.voucherInfo.year = voucher.year;
        this.voucherInfo.month = voucher.month;
        this.voucherInfo.documentMaker = voucher.documentMaker;
        this.voucherInfo.createTime = voucher.createTime;
        this.voucherInfo.auditorName = voucher.auditorName;
        this.voucherInfo.auditTime = voucher.auditTime;
        this.voucherInfo.addWay = voucher.addWay;
        if (voucher.id) {
          this.saveType = "edit";
          this.$emit("change-type", "edit");
        }
      } else {
        this.sourceRows = [];
      }
    },
    initInsertParams(insertParams) {
      if (insertParams !== null) {
        this.initVoucherInfo();
        this.initEmptyRows();
        this.toDigest(0);
        // 初始化初始数据
        this.initVoucherData = {};
        this.initVoucherData.year = insertParams.date.slice(0, 4);
        this.initVoucherData.month = insertParams.date.slice(5, 7);
        this.initVoucherData.lastDay = getLastDay(
          this.initVoucherData.year,
          this.initVoucherData.month
        );
        this.initVoucherData.date = `${this.initVoucherData.year}-${this.initVoucherData.month}-${this.initVoucherData.lastDay}`;
        this.initVoucherData.number = insertParams.number;
        // 初始化凭证数据
        this.voucherInfo.number = this.initVoucherData.number;
        this.voucherInfo.date = this.initVoucherData.date;
        this.voucherInfo.year = this.initVoucherData.year;
        this.voucherInfo.month = this.initVoucherData.month;
      }
    },
    initEvent() {
      document.onkeydown = this.initKeyDownEvent;
      document.onkeyup = this.initKeyUpEvent;
      document.onkeypress = this.initKeyPressEvent;
    },
    initAccountSet(accountSet) {
      if (this.view !== "input") {
        return;
      }
      if (this.saveType === "insert") {
        this.initInsertParams(this.insertParams);
      } else if (accountSet !== null) {
        let { currentPeriod } = accountSet;
        console.log(2222);
        this.generaterVoucherNumber(currentPeriod, true);
        // if (this.initLoadding) return
        // this.initLoadding = true
        // VoucherInputAjax.GeneraterVoucherNumber({period: currentPeriod}).then(response => {
        //   this.initLoadding = false
        //   if (response.code === 200) {
        //     this.initVoucherData = {}
        //     this.initVoucherData.year = currentPeriod.toString().slice(0, 4)
        //     this.initVoucherData.month = currentPeriod.toString().slice(4)
        //     this.initVoucherData.lastDay = getLastDay(this.initVoucherData.year, this.initVoucherData.month)
        //     this.initVoucherData.date = `${this.initVoucherData.year}-${this.initVoucherData.month}-${this.initVoucherData.lastDay}`
        //     this.initVoucherData.number = response.data
        //   }
        // })
      } else {
        this.initVoucherData = null;
      }
    }
  },
  computed: {
    ...mapState({
      digests: state => state.common.digests,
      leafAccounts: state => state.common.leafAccounts,
      userInfo: state => state.userInfo.userInfo,
      accountSet: state => state.common.accountSet,
      assistCategories: state => state.common.assistCategories
    }),
    ...mapGetters(["formatDate"]),
    hasQty() {
      let item = this.voucherDetail.find(item => {
        return item.isQuantity;
      });
      return item !== undefined;
    },
    hasLastDigest() {
      let item = this.voucherDetail.find(item => {
        return item.digest !== null;
      });
      return item !== undefined;
    },
    disgestColSpan() {
      return 4;
    },
    accountColSpan() {
      return 8;
    },
    amountColSpan() {
      return 5;
    },
    innerAmountColSpan() {
      return !this.hasQty ? 24 : 14;
    },
    qtyColSpan() {
      return 10;
    },
    sumColSpan() {
      return 12;
    },
    accountWidth() {
      return this.hasQty ? 209 : 363;
    },
    documentMaker() {
      if (this.voucherInfo.documentMaker !== "") {
        return this.voucherInfo.documentMaker;
      }
      return JSON.parse(sessionStorage.getItem("userInfo")).name;
    },
    canEdit() {
      // 0:手工录入
      // 3:结转销售成本
      // 4:计提工资
      // 7:计提社保公积金
      // 作废: -1
      if(!this.voidFlag) return false
      if (this.voucherInfo.status === 0 && this.voucher !== null) {
        if (
          this.voucher.addWay === 5 ||
          this.voucher.addWay === 8 ||
          this.voucher.addWay === 9
        ) {
          return false;
        } else {
          if (
            this.voucherType === 0 ||
            this.voucherType === 3 ||
            this.voucherType === 4 ||
            this.voucherType === 7
          ) {
            return true;
          } else {
            return true;
          }
        }
      } else if (this.voucherInfo.status === 1 || this.voucherInfo.status === 2) {
        return false
      } else if(this.voucherInfo.status === -1) {
        return false
      }else {
        return true
      }
    },
    currentFirstDayTime() {
      let { currentPeriod } = this.accountSet;
      let seconds = new Date(
        `${currentPeriod
          .toString()
          .slice(0, 4)}-${currentPeriod.toString().slice(4)}-01`
      ).getTime();
      return seconds;
    },
    currentPeriod() {
      if (this.voucherInfo.date !== "") {
        return `${this.voucherInfo.date.slice(
          0,
          4
        )}年${this.voucherInfo.date.slice(5, 7)}期`;
      } else {
        return "";
      }
    },
    chineseSumAmount() {
      let amount = "";
      if (
        this.voucherInfo.sumCreditAmount !== "" &&
        this.voucherInfo.sumCreditAmount !== "0"
      ) {
        amount = this.voucherInfo.sumCreditAmount;
      }
      if (
        (amount === "" || amount.indexOf("-") !== -1) &&
        this.voucherInfo.sumDebitAmount !== "" &&
        this.voucherInfo.sumDebitAmount !== "0"
      ) {
        amount = this.voucherInfo.sumDebitAmount;
      }
      amount = AmountUtil.parseNumber(amount, true, false, 2, 13, 2);
      return amount === "" ? "零元整" : convertCurrency(amount);
    },
    voucherDate() {
      return this.voucherInfo === null ? "" : this.voucherInfo.date;
    }
  },
  watch: {
    $route(to, from) {
      this.$store.dispatch("common/getDigests");
      this.$store.dispatch("common/getAccountSet");
    },
    type(type) {
      this.saveType = type;
    },
    voidFlag(val) {
      this.canEdit()
    },
    voucherType(voucherType) {
      if (this.voucherInfo.addWay === null) {
        this.voucherInfo.addWay = voucherType;
      }
    },
    pageIsActive(active) {
      if (active) {
        if (this.view === "input" && this.voucherDetail[0].digest === null) {
          this.toDigest(0);
        }
        this.initEvent();
      } else {
        this.$refs[`digest0`][0].blur();
      }
    },
    hasLastDigest(hasLastDigest) {
      if (!hasLastDigest) {
        this.lastDigest = null;
      }
    },
    voucher(voucher) {
      this.setVoucher(voucher);
    },
    initVoucherData: {
      handler(initVoucherData) {
        if (initVoucherData !== null && this.voucher === null) {
          this.voucherInfo.number = initVoucherData.number;
          this.voucherInfo.date = initVoucherData.date;
          this.voucherInfo.year = initVoucherData.year;
          this.voucherInfo.month = initVoucherData.month;
        }
      },
      deep: true
    },
    insertParams(insertParams) {
      this.initInsertParams(insertParams);
    },
    accountSet: {
      handler(accountSet) {
        this.initAccountSet(accountSet);
      },
      // immediate: true,
      deep: true
    },
    voucherDate(newDate, oldDate) {
      //监听日期插件改变
      if (
        !newDate ||
        newDate === oldDate ||
        this.type === "edit" ||
        this.type === "template"
      ) {
        return;
      }
      let isSamePeriod = true;
      let newYearMonth = newDate.slice(0, 7);
      if (oldDate.slice(0, 7) !== newYearMonth) {
        isSamePeriod = false;
      }
      newYearMonth = newYearMonth.replace("-", "");
      if (!isSamePeriod && this.saveType !== "insert") {
        console.log(3333);
        this.generaterVoucherNumber(newYearMonth);
      }
    }
  },
  components: {
    AmountName,
    AmountUnit,
    DigestSelect,
    AccountSelect,
    AssistSelect,
    addSubject,
    addCustomer,
    addDepartment,
    addProject,
    addStaff,
    // addStock,
    productModal,
    addSupplier,
    accountUploadBounced
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="less" scoped>
.indexArea {
  position: absolute;
  right: 8px;
  font-size: 12px;
  line-height: 1;
  color: #eee;
}
.voucherRow.canEdit {
  &:hover {
    .grid-content {
      background: rgba(0, 0, 0, 0.1);
    }
    .add-row-button,
    .delete-row-button {
      div,
      i {
        display: block;
      }
    }
  }
}
.el-form-item--small.el-form-item {
  width: 200px !important;
  .el-form-item__label {
    display: inline-block !important;
  }
}
.voucher-header {
  padding: 0 4%;
  .record_num,
  .bill_num {
    margin-left: 5px;
  }
  .cur_date {
    height: 32px;
    line-height: 32px;
    margin-right: 44px;
  }
}
.el-form-item--small.el-form-item {
  margin-bottom: 10px;
}
.voucher-footer {
  padding-left: 4%;
  padding-top: 10px;
  padding-bottom: 15px;
  padding-right: 17px;
}
.grid-content,
.voucher-body .voucher-body-title .account .el-col .grid-content {
  height: 60px;
  border-top: 1px solid #bdc3c7;
  border-left: 1px solid #bdc3c7;
}
.voucher-body-rows .active .grid-content {
  background-color: #efefef;
}
.voucher-body-title,
.voucher-body-last {
  padding-right: 17px;
}
.voucher-body-rows {
  height: 243px;
  padding-right: 17px;
  overflow-y: overlay;
  -ms-overflow-style: none;
  overflow: -moz-scrollbars-none;
}
.voucher-body-rows::-webkit-scrollbar {
  // width: 0 !important;
}
.voucher-body-rows .el-row:first-child .grid-content {
  border-top: none;
}
.voucher-body-title .grid-content {
  line-height: 60px;
  text-align: center;
  font-weight: 700;
}
.voucher-body-last .grid-content,
.voucher-body-last .grid-content,
.voucher-body .voucher-body-title .account .el-col .grid-content,
.voucher-body-title .grid-content,
.amount-title,
.account-qty input {
  border-bottom: 1px solid #bdc3c7;
}
.voucher-body .el-col:first-child .grid-content {
  border: none;
  background: none;
}
.voucher-body .el-col:last-child .grid-content {
  border: none;
  background: none;
}
.add-row-button {
  position: relative;
  display: flex;
  align-items: center;
  flex-direction: column;
  justify-content: space-around;
}
.add-row-button i {
  cursor: pointer;
  display: none;
}
.delete-row-button {
  color: red;
  display: flex;
  align-items: center;
}
.delete-row-button i {
  cursor: pointer;
  display: none;
  margin-left: 5px;
}
.voucher-body {
  font-size: 13px;
  color: #555;
}
.voucher-body-title {
  font-weight: 700;
}
.last-amount-grid {
  border-right: 1px solid #bdc3c7;
}
.amount-title {
  height: 30px;
  line-height: 30px;
  overflow: hidden;
}
.voucher-body-title .amount-title {
  font-weight: 700;
}
.account-select {
  height: 60px;
}
.assist-select {
  height: 60px;
}
.qty-content {
  border-left: 1px solid #bdc3c7;
  height: 60px;
}

/* 数量核算 */
.account-qty,
.account-price {
  display: block;
  border: none;
  height: 30px;
  width: 100%;
  font-size: 12px;
  line-height: 30px;
}
.account-qty span {
  padding-bottom: 3px;
}
.account-qty .qty-label {
  margin-left: 10px;
  width: 30px;
  line-height: 20px;
  height: 20px;
}
.account-qty input {
  margin: 0 5px;
  width: 50%;
  border: none;
  border-bottom: 1px solid #bdc3c7;
  outline: none;
  background: none;
}
.account-qty .qty-unit {
  width: 30px;
  line-height: 20px;
  height: 20px;
}
</style>
<style lang="less">
@import "../../style/base.less";
.voucher-header {
  .record_span,
  .bill_span {
    position: relative;
    .voucher-number {
      width: 28px;
      height: 26px;
      font-size: 12px;
      padding: 0 10px;
      border: 1px solid #eee;
      color: #606266;
      outline: none;
      &:focus {
        border: 1px solid #39c;
      }
    }
    .add_record,
    .add_bill,
    .reduce_record,
    .reduce_bill {
      cursor: pointer;
      position: absolute;
      display: block;
      width: 16px;
      height: 14px;
    }
    .add_record,
    .add_bill {
      right: 2px;
      top: -4px;
      .mixin-bis(16px;14px;-385px;-220px);
    }
    .reduce_record,
    .reduce_bill {
      right: 2px;
      bottom: -6px;
      .mixin-bis(16px;14px;-435px;-224px);
    }
  }
  .account_header_bill {
    width: 300px !important;
  }
  .account_header_code,
  .account_header_bill {
    .el-form-item__label {
      display: inline-block !important;
    }
    .el-form-item__content {
      width: 150px !important;
      position: relative;
      .upload-bounced {
        position: absolute;
        right: 0;
        background: #fff;
        z-index: 999;
        padding: 20px;
        width: 300px;
        border: 1px solid #ddd;
        border-radius: 10px;
        height: 350px;
      }
    }
  }
  .account_header_date {
    margin-left: 44px;
    min-width: 150px;
    .el-form-item__label {
      display: inline-block !important;
    }
    .el-date-editor.el-input {
      width: 120px;
      .el-input__inner {
        height: 28px;
        padding-left: 10px;
      }
      .el-input__prefix {
        left: 85px;
      }
    }
  }
}
.voucher-main {
  position: relative;
}
.voucher_status {
  position: absolute;
  right: 25px;
  top: 20px;
  z-index: 1;
}
.voucher_status.hasAudit {
  .mixin-bis(105px;90px;-492px;-10px);
}
.voucher_status.hasCheckOut {
  .mixin-bis(105px;90px;-492px;-110px);
}
.voucher_end_status {
  position: absolute;
  right: 54px;
  top: 107px;
  z-index: 1;
}
.voucher_end_status.hasInvisible {
  /*无形资产*/
  .mixin-bis(90px;90px;-725px;-120px);
}
.voucher_end_status.hasAssets {
  /*长期待摊销*/
  .mixin-bis(90px;90px;-725px;-232px);
}
.voucher_end_status.hasFixed {
  /*固定资产*/
  .mixin-bis(90px;90px;-725px;-10px);
}
.chinese-sum-amount {
  font-size: 13px;
  font-weight: 700;
  padding: 23px 10px;
}
.bill-curser {
  cursor: pointer;
}
</style>

```

```js
<template>
  <div class="voucher_Input">
    <div class="voucher_InputCon">
      <div class="voucher_top_Con">
        <div class="top_left" :class="{'hover_keyboard': showTip}" @mouseover="showTip = true" @mouseout="showTip = false">
          <i class="icon el-icon-bell"></i>
          <span class="keyboard_tip">“亲，按回车键或者Tab，能快速选择单元格哟！”</span>
          <div class="shortcut_container" v-show="showTip">
            <key-shortcut/>
          </div>
        </div>
        <div class="top_right">
          <!-- <el-button tabindex="-1" class="btn btn-ordinary btn_pre btn-common" @click="copyVoucher"><span>冲销</span></el-button>
          <el-button tabindex="-1" class="btn btn-ordinary btn_pre btn-common" @click="copyVoucher"><span>复制</span></el-button>
          <el-button tabindex="-1" class="btn btn-ordinary btn_pre btn-common" @click="pasteVoucher"><span>粘贴</span></el-button> -->
          <el-button tabindex="-1" class="btn btn-ordinary btn_pre btn-common" :disabled="!hasPrev" @click="queryVoucher('previous')"><i class="icon_pre"></i><span>上一张</span></el-button>
          <el-button tabindex="-1" class="btn btn-ordinary btn_next btn-common" :disabled="!hasNext" @click="queryVoucher('next')"><span>下一张</span><i class="icon_next"></i></el-button>
          <el-button tabindex="-1" class="btn btn-main more_voucher" v-show="view === 'input'" @click="moreVoucher">更多凭证</el-button>
        </div>
      </div>
      <div class="voucher_main_Con">
        <div class="keep_title"><span class="keep_con"><span>记账凭证</span><div class="first_line"></div><div class="second_line"></div></span>
          <div v-if="!voidFlag" style="background: #e89e42; color: #fff; padding: 10px; padding: 0px 5px;position: absolute;right: 250px;height: 30px;line-height: 30px;top: 73px;">
            该凭证已作废，无法进行修改！
          </div>
        </div>
        <voucher
        :voucher="voucher"
        :view="view"
        ref="voucher"
        :type="type"
        :pageIsActive="isPageActive"
        :insertParams="insertParams"
        :uploadFiles="uploadFiles"
        :voidFlag="voidFlag"
        @change-type="changeType"
        @after-save="afterSave($event)"></voucher>
      </div>
      <div ref="voucherBottom" class="voucher_Input_bottom_Con">
        <el-button class="btn btn-main btn_template white" :class="{'isClick': isClickTemplate}" v-show="view === 'input' && showSave" @click.stop="clickTempBtn">
          <span>模板</span>
          <i class="icon_pre"></i>
          <div v-if="isClickTemplate" class="template_con" v-clickoutside="clickTempBtn">
            <ul>
              <li @click="userTemplate">使用模板</li>
              <li @click="saveAsTemplate">存为模板</li>
            </ul>
          </div>
        </el-button>
        <el-button tabindex="-1" v-if="showSave && view === 'detail'" class="btn btn-main white" @click="saveAsTemplate">存为模板</el-button>
        <el-button tabindex="-1" v-if="showAudit" class="btn btn-main audit white" @click="auditVoucher()">审核</el-button>
        <el-button tabindex="-1" v-if="showRevokeAudit" class="btn btn-main revokeAudit white" @click="revokeAudit()">反审核</el-button>
        <el-button tabindex="-1" v-if="view === 'input' && showSave" class="btn btn-main save_insert white" @click="saveVoucher('saveAndNew')">保存并新增（ F12 )</el-button>
        <el-button tabindex="-1" v-if="showSave" class="btn btn-main save white" @click="saveVoucher('save')">保存（ Ctrl+S)</el-button>
        <el-button tabindex="-1" v-if="showDelete" class="btn btn-main audit white" style="background-color: #f56c6c;border-color: #f56c6c;" @click="deleteVisible = true">删除</el-button>
        <el-dropdown @command="handleClickDropdown" v-if="view === 'detail' || type == 'edit'">
          <el-button class="btn btn-main audit white">
            更多<i class="el-icon-arrow-down el-icon--right"></i>
          </el-button>
          <el-dropdown-menu slot="dropdown">
            <el-dropdown-item command="copy">复制</el-dropdown-item>
            <!-- <el-dropdown-item command="void" v-if="this.voidFlag">作废</el-dropdown-item>
            <el-dropdown-item command="cancelVoid" v-else>取消作废</el-dropdown-item> -->
            <el-dropdown-item command="cx">冲销</el-dropdown-item>
          </el-dropdown-menu>
        </el-dropdown>
      </div>
    </div>
    <voucher-dialog
      v-if="deleteVisible"
      v-loading="loading"
      :title="'删除'"
      :width="'486px'"
      :mainText="'删除后不可恢复，并产生断号，您确认删除此凭证吗？'"
      :dialogVisible="deleteVisible"
      @click-cancel="deleteVisible = false"
      @click-sure="deleteClickSure"
    />
    <save-template
      v-if="validateTemplateFlag"
      :dialogVisible="validateTemplateFlag"
      :data="templateDetail"
      @click-cancel-form="validateTemplateFlag = false"
      @click-sure-form="saveTemplate"
    />
    <template-list
      ref="tempList"
      v-if="templateListFlag"
      :dialogVisible="templateListFlag"
      @add-new-template="addNewTemplate"
      @use-template="selectTemplate"
      @click-edit="editTemplate"
      @click-cancel-form="closeTemplateList"
    />
    <add-template
      v-if="addTemplateFlag"
      :tempDialogTitle="tempDialogTitle"
      :templateId="tempDialogId"
      :dialogVisible="addTemplateFlag"
      @click-cancel-form="cancelAddTemplate"
      @save-template-success="saveTempSuccess"
    />
  </div>
</template>
<script>
import voucher from './Voucher'
import AddTemplate from '@/views/EnterpriseCenter/KeepAccount/VoucherInput/subpage/subpage/AddTemplate'
import KeyShortcut from './Voucher/KeyShortcut'
import VoucherInputAjax from '@/api/VoucherInput/VoucherInput'
import VoucherDialog from '@/components/Dialog/Dialog'
import VoucherQueryAjax from '@/api/VocherQuery/VocherQuery'
import { message } from '@/components/Message/Message'
import SaveTemplate from '@/views/EnterpriseCenter/KeepAccount/VoucherInput/subpage/subpage/SaveTemplate'
import { validateTemplateAction } from '@/views/EnterpriseCenter/KeepAccount/VoucherInput/subpage/subpage/Actions.js'
import TemplateList from '@/views/EnterpriseCenter/KeepAccount/VoucherInput/subpage/subpage/TemplateList'
import { clickoutside } from '@/tools/tools'
// import { mapState, mapActions, mapMutations } from 'vuex'
import { mapState, mapGetters } from 'vuex'

export default {
  directives: {clickoutside},
  props: {
    view: {
      type: String,
      default: 'input' // input or detail
    },
    refreshPage: {
      type: String,
      default: ''
    },
    // uploadFiles: {
    //   type: Array,
    //   default: () => [],
    // },
  },
  data () {
    return {
      loading: false,
      showTip: false,
      voucher: null,
      preVoucher: null,
      preVoucherId: null,
      initVoucherData: null,
      insertParams: null,
      type: '',
      deleteVisible: false,
      validateTemplateFlag: false,
      isClickTemplate: false,
      templateListFlag: false,
      addTemplateFlag: false,
      tempDialogTitle: '',
      tempDialogId: null,
      enableEvent: true,
      templateDetail: [],
      uploadFiles:[],
      voidFlag: true, // true 取消作废状态  false 作废状态
    }
  },
  created () {
    if (this.view === 'input') {
      this.initAddNew()
    } else {
      this.type = 'edit'
      let params = this.$route.params
      sessionStorage.setItem('newVoucherId', params.voucherId)
      // console.log(params);
      if (params && params.voucherId && params.isEdit) {
        this.queryVoucherById(params.voucherId)
      } else {
        VoucherInputAjax.QueryNewVoucher({number: 1}).then(response => {
          if (response.code === 200) {
            let voucher = response.data.length > 0 ? response.data[0] : null
            if (voucher !== null) {
              this.queryVoucherById(voucher.id)
            }
          }
        })
      }
    }
  },
  mounted() {
  },
  computed: {
    ...mapGetters(['getTodoById']),
    ...mapState({
      accountSet: (state) => state.common.accountSet
    }),
    hasNext () {
      if (this.voucher) {
        return this.voucher.nextId > 0
      } else if (this.preVoucher) {
        return this.preVoucher.nextId > 0
      }
      return false
    },
    hasPrev () {
      return this.preVoucherId !== null
    },
    isPageActive () {
      if (this.view === 'input') {
        return this.refreshPage === 'voucherInput' && this.enableEvent
      } else {
        return this.refreshPage === 'voucherInfo' && this.enableEvent
      }
    },
    showAudit () {
      return this.getTodoById('voucher-query/audit') && this.accountSet && this.accountSet.enableAudit && this.voucher && this.voucher.status === 0 && this.view === 'detail'
    },
    showRevokeAudit () {
      return this.getTodoById('voucher-query/audit') && this.voucher && this.voucher.status !== 1 && this.voucher.audit && this.view === 'detail'
    },
    showSave () {
      return this.voucher === null || this.voucher.status === 0
    },
    showDelete () {
      return this.voucher !== null && this.view === 'detail' && this.voucher.status !== 1
    }
  },
  methods: {
    // 更多
    handleClickDropdown(val) {
      console.log(val,'val')
      if(val == 'copy'){//复制
        VoucherQueryAjax.VoucherCopy({ ids: [this.voucher.id?this.voucher.id: this.$route.params.voucherId] }).then(res => {
          if (res.code === 200) {
            message({
              message: "凭证已复制，请稍后刷新查看！",
              type: "success"
            });
            this.queryVoucherById(res.data[0].id)
          } else {
            message({
              message: res.message,
              type: "error"
            });
          }
        });
      }else if(val == 'cx') {//冲销
        VoucherQueryAjax.VoucherWriteOff({ id: this.voucher.id?this.voucher.id: this.$route.params.voucherId }).then(res => {
          if (res.code === 200) {
            message({
              message: "冲销凭证已创建，请稍后刷新查看！",
              type: "success"
            });
            this.queryVoucherById(res.data[0].id)
          } else {
            message({
              message: res.message,
              type: "error"
            });
          }
        });

      }else if(val == 'void') {
        // this.voidFlag = !this.voidFlag
        VoucherQueryAjax.VoucherVoidAPI({id: [this.voucher.id?this.voucher.id: this.$route.params.voucherId] }).then(res => {
          if (res.code === 200) {
            message({
              message: "凭证已作废，请稍后刷新查看！",
              type: "success"
            });
            this.queryVoucherById(res.data[0].id)
          } else {
            message({
              message: res.message,
              type: "error"
            });
          }
        })
      }else if(val == 'cancelVoid') {
        // 发送取消作废的请求
        this.voidFlag = !this.voidFlag
        VoucherQueryAjax.VoucherCancelVoidAPI({id: [this.voucher.id?this.voucher.id: this.$route.params.voucherId] }).then(res => {
          if (res.code === 200) {
            message({
              message: "凭证已取消作废，请稍后刷新查看！",
              type: "success"
            });
            this.queryVoucherById(res.data[0].id)
          } else {
            message({
              message: res.message,
              type: "error"
            });
          }
        })
      }
    },
    afterSave (event) {
      if (event.success && !event.saveAndNew) {
        this.setCurrentVoucher(event.voucher)
        this.type = 'edit'
      } else if (event.success) {
        this.initAddNew()
        this.type = 'add'
      }
    },
    changeType (type) {
      this.type = type
    },
    initAddNew () {
      this.$store.dispatch('common/getAccountSet')
      this.voucher = null
      this.preVoucher = null
      this.preVoucherId = null
      this.initVoucherData = null
      this.insertParams = null
      this.$nextTick(() => {
        this.$refs.voucher.initVoucherInfo()
        this.$refs.voucher.initEmptyRows()
      })
      if (this.$route.params.afterVoucher && this.$route.params.voucherId) {
        this.type = 'insert'
        this.insertParams = this.$route.params
      } else {
        this.type = 'add'
      }
      // 查询最新凭证详情
      VoucherInputAjax.QueryNewVoucher({number: 1}).then(response => {
        if (response.code === 200) {
          this.preVoucher = response.data.length > 0 ? response.data[0] : null
          if (this.preVoucher !== null) {
            this.preVoucherId = this.preVoucher.id
          }
        }
      })
    },
    queryVoucher (type) {
      let id = null
      if (type === 'previous') {
        if (this.preVoucherId !== null) {
          id = this.preVoucherId
        }
      } else if (this.voucher !== null) {
        id = this.voucher.nextId
      }
      if (id === null) {
        return
      }
      this.$refs.voucher.clearFileArrFun()
      this.queryVoucherById(id)
    },
    queryVoucherById (id) {
      VoucherInputAjax.QueryVoucherDetail({voucherId: id}).then(response => {
        if (response.code === 200) {
          if (response.data) {
            this.uploadFiles = response.data.voucherAttachmentList
            this.$refs.voucher.setUploadFiles(response.data.voucherAttachmentList,response.data)
            this.setCurrentVoucher(response.data)
            this.type = 'edit'
            this.type = 'detail'
            // 获取到订单的详情信息，看看是否是作废订单
            console.log("获取凭证详情");
            this.voidFlag = response.data.cancellation == 0 ? false : true
          } else {
            message({
              message: '该条凭证信息已删除！',
              type: 'error'
            })
            setTimeout(() => {
              let objTab = document.getElementsByClassName('el-tabs__item is-top is-closable')
              for (let i = 0; i < objTab.length; i++) {
                if (objTab[i].textContent === '凭证信息') {
                  objTab[i].firstElementChild.click()
                }
              }
            }, 1500)
          }
        }
      })
    },
    setCurrentVoucher (voucher) {
      this.voucher = voucher
      this.preVoucher = null
      this.preVoucherId = this.voucher.previousId
    },
    moreVoucher () {
      this.$router.push({name: 'voucherQuery'})
    },
    saveVoucher (type) {
      console.log(type);
      console.log(type !== 'save');
      this.$refs.voucher.save(type !== 'save')
    },
    saveAsTemplate () {
      if (validateTemplateAction(this, this.$refs.voucher.packageData().voucherDetailFormList)) {
        this.templateDetail = this.$refs.voucher.packageData().voucherDetailFormList
        this.validateTemplateFlag = true
      }
    },
    copyVoucher () {
      this.$refs.voucher.copyFun()
    },
    pasteVoucher () {
      this.$refs.voucher.pasteFun()
    },
    saveTemplate (templateName) {
      this.validateTemplateFlag = false
    },
    deleteClickSure () { // 删除该凭证
      this.loading = true
      let objTab = document.getElementsByClassName('el-tabs__item is-top is-active is-closable')[0]
      let currentTab = objTab.getElementsByClassName('el-icon-close')[0]
      let ids = []
      ids.push(this.voucher.id)
      VoucherQueryAjax.VoucherDelete({ids: ids}).then(res => {
        if (res.code === 200) {
          message({
            message: '凭证删除成功！',
            type: 'success'
          })
          this.loading = false
          this.deleteVisible = false
          currentTab.click()
          // let tabsFlagArr = localStorage.getItem('tabsFlagArr')
          this.$router.go(-2)
          // if (tabsFlagArr != null && JSON.parse(tabsFlagArr).length > 0) {
          //   this.$router.go(-2)
          // }
        } else {
          message({
            message: res.message,
            type: 'error'
          })
        }
      })
    },
    auditVoucher () {
      let postData = {
        ids: [this.voucher.id]
      }
      VoucherQueryAjax.VoucherToExamine(postData).then(response => {
        if (response.code === 200) {
          this.$message({
            message: '凭证审核成功！',
            type: 'success'
          })
          this.queryVoucherById(this.voucher.id)
        }
      })
    },
    revokeAudit () {
      let postData = {
        ids: [this.voucher.id]
      }
      VoucherQueryAjax.VoucherAntiAudit(postData).then(response => {
        if (response.code === 200) {
          this.$message({
            message: '凭证反审核成功！',
            type: 'success'
          })
          this.queryVoucherById(this.voucher.id)
        }
      })
    },
    clickTempBtn () {
      this.isClickTemplate = !this.isClickTemplate
    },
    userTemplate () {
      this.templateListFlag = !this.templateListFlag
    },
    selectTemplate (template) {
      this.$refs.voucher.useTemplate(template)
      this.templateListFlag = false
    },
    cancelAddTemplate () {
      this.addTemplateFlag = false
    },
    addNewTemplate () {
      this.addTemplateFlag = true
      this.tempDialogTitle = '新增模板'
      this.tempDialogId = null
    },
    editTemplate (id) {
      this.addTemplateFlag = true
      this.tempDialogTitle = '编辑模板'
      this.tempDialogId = id
    },
    saveTempSuccess () {
      this.addTemplateFlag = false
      if (this.$refs.tempList) {
        this.$refs.tempList.init()
      }
    },
    closeTemplateList () {
      this.templateListFlag = false
    }
  },
  watch: {
    isPageActive () {
      if (this.voucher !== null && this.view === 'detail') {
        // this.queryVoucherById(this.voucher.id)
      }
    },
    $route: {
      handler: function (val, oldVal) {
        if (val !== null && this.view === 'input') {
          if (val.params.afterVoucher && val.params.voucherId && val.name === 'voucherInput') {
            this.type = 'insert'
            this.insertParams = val.params
          } else if (val.params.isAdd) {
            this.type = 'add'
            this.initAddNew()
          }
        } else if (val !== null && this.view === 'detail' && val.name === 'voucherInfo') {
          if (val.params.voucherId && val.params.isEdit) {
            sessionStorage.setItem('newVoucherId', val.params.voucherId)
            this.type = 'edit'
            this.queryVoucherById(val.params.voucherId)
          } else {
            this.type = 'edit'
            this.queryVoucherById(sessionStorage.newVoucherId)
          }
        }
      },
      deep: true
    },
    accountSet (accountSet) {
    },
    templateListFlag (templateListFlag) {
      this.enableEvent = !templateListFlag // 当显示模板列表时禁用凭证录入的快捷键
    }
  },
  components: {
    voucher,
    KeyShortcut,
    VoucherDialog,
    SaveTemplate,
    AddTemplate,
    TemplateList
  }
}
</script>
<style lang="less" scoped>
@import '../../style/base.less';
.voucher_main_Con{
  color: #555;
  margin: 0px 15px;
  display: flex;
  flex-direction: column;
  height: 520px;
  margin-top: 4px;
  box-shadow:0 0 4px 0 rgba(0,0,0,0.30);
}
.keep_title{
  height: 58px;
  line-height: 58px;
  text-align: center;
  overflow: hidden;
  .keep_con{
    display: block;
    width: 80px;
    height: 28px;
    position: relative;
    margin: 15px auto;
    span{
      font-size:20px;
      display: block;
      width: 80px;
      height: 28px;
      line-height: 28px;
    }
    .first_line,.second_line{
      position: absolute;
      width: 80px;
      height: 1px;
      background: #555;
    }
    .first_line{
      bottom: 2px;
    }
    .second_line{
      bottom: 0px;
    }
  }
}
.voucher_top_Con{
  .top_right{
    button.el-button.is-disabled{
      background: #f9f9f9!important;
      border:1px solid #eeeeee!important;
      span{
        color: #999!important;
        font-size:13px;
      }
      .icon_pre{
        .mixin-bis(5px;10px;-369px;-10px);
      }
      .icon_next{
        .mixin-bis(5px;10px;-396px;-10px);
      }
    }
  }
}
.voucher_top_Con{
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  height: 52px;
  .top_left{
    position: relative;
    display: flex;
    align-items: center;
    margin-left: 20px;
    .icon_keyboard{
      display: inline-block;
      .mixin-bis(36px;28px;-386px;-89px);
    }
    .keyboard_tip{
      margin-left: 5px;
      cursor: pointer;
      .mixin-sc(13px;#999);
    }
    .shortcut_container{
      position: absolute;
      left: -5px;
      top: 22px;
      z-index: 9999;
      width: 328px;
      height: 365px;
    }
  }
  .hover_keyboard{
    .icon_keyboard{
      .mixin-bis(36px;28px;-437px;-89px);
    }
    .keyboard_tip{
      color:#5478fb;
      text-decoration: underline;
    }
  }
  .top_right{
    display: flex;
    margin-right: 15px;
    button{
      margin: 0;
      font-size: 13px;
      margin-left:10px;
    }
    .btn_pre{
      .icon_pre{
        margin-right: 6px;
        display: inline-block;
        .mixin-bis(5px;10px;-369px;-30px);
      }
    }
    .btn_pre:hover{
      .icon_pre{
        .mixin-bis(5px;10px;-369px;-10px);
      }
    }
    .btn_next{
      .icon_next{
        display: inline-block;
        margin-left: 6px;
        .mixin-bis(5px;10px;-396px;-30px);
      }
    }
    .btn_next:hover{
      .icon_next{
        .mixin-bis(5px;10px;-396px;-10px);
      }
    }
    .btn-common{
      padding: 0px 13px;
      display: flex;
      align-items: center;
      span{
        display: inline-block;
        span{
          font-size: 13px;
        }
      }
    }
  }
}
.main_Con{
  margin: 0px 15px;
  display: flex;
  height: 500px;
  margin-top: 4px;
  box-shadow:0 0 4px 0 rgba(0,0,0,0.30);
}
.bottom_Con{
  width: 100%;
  display: flex;
  justify-content: flex-end;
  height: 52px;
  align-items: center;
  button{
    margin-left: 10px;
  }
  .save{
    margin-right: 15px;
  }
}
.voucher_Input_bottom_Con{
  width: 100%;
  display: flex;
  justify-content: flex-end;
  height: 52px;
  align-items: center;
  padding-right: 15px;
  button{
    margin-left: 10px;
  }
  button.el-button.white{
    span{
      .mixin-sc(13px;#fff)
    }
  }
  button.btn_template{
    width: 68px;
    position: relative;
    span {
      position: relative;
      span {
        text-align: left;
        margin-left: 2px;
      }
      i.icon_pre {
        display: inline-block;
        position: absolute;
        top: 9px;
        right: 12px;
        .minxin-triangles(4px;4px;transparent;transparent;#fff;transparent)
      }
      .template_con {
        position: absolute;
        bottom: 34px;
        left: -14px;
        .mixin-bis(94px;80px;-492px;-306px);
        ul{
          width: 100%;
          height: 80%;
          margin-top: 15px;
          li{
            .mixin-sc(14px;#555);
            height: 30px;
          }
          li:hover{
            .mixin-sc(14px;#5478fb);
          }
        }
      }
    }
  }
  button.save_insert{
    width: 136px;
    span{
      font-size: 13px;
    }
  }
}
.voucher_Input{
  background: #fff;
  width: 100%;
  .voucher_InputCon{
    margin: 0 auto;
    display: flex;
    flex-direction: column;
    width: 1202px;
    height: 612px;
    .main_Con{
      margin: 0px 15px;
      display: flex;
      height: 500px;
      margin-top: 4px;
      box-shadow:0 0 4px 0 rgba(0,0,0,0.30);
    }
    .bottom_Con{
      width: 100%;
      display: flex;
      justify-content: flex-end;
      height: 52px;
      align-items: center;
      button{
        margin-left: 10px;
      }
      .save{
        margin-right: 15px;
      }
    }
  }
}
</style>

```





### 3



```js
<template>
  <div class="main-subject profitForm">
    <div class="main-top">
      <div class="dateChange">
        <el-date-picker
          v-if="monthDateShow"
          :picker-options="disabledDate"
          style="width:170px; padding-right: 0;float: left"
          :editable="false"
          type="year"
          format="yyyy年"
          value-format="yyyy"
          :clearable="false"
          placeholder="选择期间"
          v-model="monthReportDate"
          @change="changeMonthReportDate"
        >
        </el-date-picker>
        <em class="el-icon-refresh-salf" circle @click="refreshList"></em>
      </div>
      <div class="btn-group">
        <el-button class="btn btn-main btn-import" @click="changeDetailsFlag"
          >{{ detailsTitle }}</el-button
        >
        <el-button class="btn btn-main btn-import" @click="printBtn"
          >打印</el-button
        >
        <el-button class="btn btn-main btn-import" @click="exportBtn"
          >导出</el-button
        >
      </div>
    </div>
    <div class="main-content customer-box">
      <el-col :span="2">
        <div class="date-selection">
    <p class="moth-title" @click="pageUp">
      <!-- {{ activeYear }} -->
      <i
        :class="
          yearArray[0] <= startYear
            ? 'pack-up-defaut el-icon-arrow-up'
            : 'pack-up el-icon-arrow-up'
        "
      />
    </p>
    <div class="item-moth">
      <ul>
        <li class="disable-text">12</li>
        <li class="active-item">34</li>
        <li class="">56</li>
        <li
          v-for="(item, index) in yearArray"
          :class="[
            item < startYear || item > endYear ? 'disable-text' : '',
            activeYear === item ? 'active-item' : ''
          ]"
          @click="item.disabled ? null : clickMoth(item)"
          :key="index"
        >
          <p>{{ item }}年</p>
        </li>
      </ul>
    </div>
    <!-- <p>
      <i
        @click="pageDown"
        style="font-size: 24px;"
        :class="Number(activeYear)>=endYear?'pack-up-defaut el-icon-arrow-down':'pack-up el-icon-arrow-down'"
      ></i>
    </p> -->
    <p class="moth-title" @click="pageDown">
      <!-- {{ Number(activeYear) + 1 }} -->
      <i
        :class="
          yearArray[yearArray.length - 1] >= endYear
            ? 'pack-up-defaut el-icon-arrow-down'
            : 'pack-up el-icon-arrow-down'
        "
      />
    </p>
  </div>
        </el-col>
      <el-col :span="22">
      <div class="table-main" style="height: 100%">
        <el-table
          ref="mainTable"
          class="periodTable"
          :data="filterTableDate"
          border
          highlight-current-row
          height="98%"
          emptyText="暂无数据"
          :row-class-name="tableRowClassName"
        >
          <el-table-column :resizable="false" min-width="200px" align="left" label="项目">
            <template slot-scope="scope">
              <div>
                <span
                  :style="{
                    marginLeft: marginFn(scope.row),
                    fontWeight: scope.row.indentLevel === 0 ? 'bold' : 'normal'
                  }"
                  >{{ scope.row.expenseItemName }}</span
                >
              </div>
            </template>
          </el-table-column>
          <el-table-column min-width="200px" align="right" prop="janAmount" label="1月" >
            <template slot-scope="scope">
                <span>{{ numberFormat(scope.row.janAmount) }}</span>
            </template>
          </el-table-column>
          <el-table-column min-width="200px" align="right" prop="febAmount" label="2月" >
            <template slot-scope="scope">
                <span>{{ numberFormat(scope.row.febAmount) }}</span>
            </template>
          </el-table-column>
          <el-table-column min-width="200px" align="right" prop="marAmount" label="3月" >
            <template slot-scope="scope">
                <span>{{ numberFormat(scope.row.marAmount)  }}</span>
            </template>
          </el-table-column>
          <el-table-column min-width="200px" align="right" prop="aprAmount" label="4月" >
            <template slot-scope="scope">
                <span>{{ numberFormat(scope.row.aprAmount) }}</span>
            </template>
          </el-table-column>
          <el-table-column min-width="200px" align="right" prop="mayAmount" label="5月" >
            <template slot-scope="scope">
                <span>{{ numberFormat(scope.row.mayAmount) }}</span>
            </template>
          </el-table-column>
          <el-table-column min-width="200px" align="right" prop="junAmount" label="6月" >
            <template slot-scope="scope">
                <span>{{ numberFormat(scope.row.junAmount) }}</span>
            </template>
          </el-table-column>
          <el-table-column min-width="200px" align="right" prop="julAmount" label="7月" >
            <template slot-scope="scope">
                <span>{{ numberFormat(scope.row.julAmount) }}</span>
            </template>
          </el-table-column>
          <el-table-column min-width="200px" align="right" prop="augAmount" label="8月" >
            <template slot-scope="scope">
                <span>{{ numberFormat(scope.row.augAmount) }}</span>
            </template>
          </el-table-column>
          <el-table-column min-width="200px" align="right" prop="sepAmount" label="9月" >
            <template slot-scope="scope">
                <span>{{ numberFormat(scope.row.sepAmount) }}</span>
            </template>
          </el-table-column>
          <el-table-column min-width="200px" align="right" prop="octAmount" label="10月" >
            <template slot-scope="scope">
                <span>{{ numberFormat(scope.row.octAmount) }}</span>
            </template>
          </el-table-column>
          <el-table-column min-width="200px" align="right" prop="novAmount" label="11月" >
            <template slot-scope="scope">
                <span>{{ numberFormat(scope.row.novAmount) }}</span>
            </template>
          </el-table-column>
          <el-table-column min-width="200px" align="right" prop="decAmount" label="12月" >
            <template slot-scope="scope">
                <span>{{ numberFormat(scope.row.decAmount) }}</span>
            </template>
          </el-table-column>
          <el-table-column min-width="200px" align="right" prop="yearAmountStr" label="合计" >
            <template slot-scope="scope">
                <span>{{ scope.row.yearAmountStr }}</span>
            </template>
          </el-table-column>
        </el-table>
      </div>
    </el-col>

    </div>
  </div>
</template>

<script>
import StatementAjax from "@/api/statement/statement";
import { printExport } from "@/api/axios.config";
import { mapGetters } from "vuex";
import DateYearChoose from "@/components/dateSelection/dateYearChoose";
let page;
export default {
  props: {
    refreshPage: {
      type: String,
      default: ""
    }
  },
  components: {
    DateYearChoose
  },
  data() {
    return {
      monthDateShow: true,
      Period: "", // 当前会计期间
      startPeriod: "", // 启用期间
      end: "", // 凭证记账最大日期
      tableDate: null,
      filterTableDate: null,
      disabledDate: {
        disabledDate: time => {
          return (
            new Date(page.startPeriod).getTime() >= time.getTime() ||
            new Date(page.end).getTime() <= time.getTime()
          );
        }
      },
      monthReportDate: "",
      querySchemer: {
        date: ""
      },
      detailsTitle: '显示费用明细',
      detailFlag: true,
      timeInterval: {
        start:null,
        end:null,
      },
      formTiem: {},

      yearArray: [],
      startYear: '',
      endYear: ''
    };
  },
  created() {
    page = this;
    this.init();
  },
  watch: {
    $route(to, from) {
      if (from.name === "checkoutTest" && to.query.currentPeriod) {
        this.monthReportDate = to.query.currentPeriod;
        this.monthReportDateDef = this.monthReportDate;
        this.AssetsDebtReportSearchForm.period = this.monthReportDate;
        this.assetsDebtList();
      }
    },
    refreshPage(newVal, oldVal) {
      if (newVal === "profitForm") {
        this.queryPeriod("clickTab");
      }
    },
    deep: true
  },
  computed: {
    ...mapGetters(["getTodoById", "getOrder"])
  },
  methods: {
    init() {
      init.bind(this)();
      async function init() {
        console.log(this);
        await this.queryPeriod();
      }
    },
    queryProfitList(a) {
      // 查询费用统计表
      let val = a.split("-")[0];
      // console.log('重制查询费用统计表',val);
      StatementAjax.ExpenseList(val).then(res => {
        if (res.code === 200) {
          // 发送请求
          if (res.code == 200) {
            this.tableDate = res.data;
            this.filterTableDate = res.data;
          }
        }
      });
    },
    async queryPeriod(fn) {
      // 会计期间
      return new Promise((resolve, reject) => {
        StatementAjax.QueryAccountPeriod().then(res => {
          if (res.code === 200) {
            this.Period = res.data.currentPeriod;
            this.monthReportDate = res.data.currentPeriod;
            this.startPeriod = res.data.start;
            this.end = res.data.end;
            this.timeInterval.start = res.data.start;
            this.timeInterval.end = res.data.end;
            console.log(this.timeInterval);
            this.querySchemer.date = res.data.currentPeriod.split("-")[0];
            resolve(res.data);
            let val = `date=${this.Period}`;
            this.queryProfitList(val);
          }
        });
      });
    },
    refreshList() {
      this.queryPeriod()
    },
    //左侧年份
    selectDateGetList(val) {
      this.timeInterval.start = val;
      this.timeInterval.end = val;
      // 搜索
      this.queryPeriod()
      // this.clickSearch();
    },
    changeDetailsFlag() {
      this.detailsFlag = !this.detailsFlag
      this.detailsTitle =  this.detailsFlag ? '显示费用明细' : '隐藏费用明细'
      if(this.detailsFlag) {
        this.filterTableDate = this.tableDate.filter((item) => item.indentLevel == 0)
      }else {
        this.filterTableDate = this.tableDate
      }
    },
    tableRowClassName({row, rowIndex}) {
        if (row.indentLevel == 0) {
          return 'highline-row';
        }
        return '';
    },
    changeMonthReportDate(val) {
      console.log(val);
      if (val === null) return;
      this.querySchemer.date = val;
      this.Period = val;
      this.monthReportDateDef = this.monthReportDate;
      let newVal = `date=${val}`;
      this.queryProfitList(newVal);
    },
    marginFn(val) {
      if (val.indentLevel === 1) {
        return "20px";
      }
      if (val.indentLevel === 2) {
        return "40px";
      }
      if (val.indentLevel === 3) {
        return "60px";
      }
      if (val.indentLevel === 4) {
        return "80px";
      }
      if (val.indentLevel === 5) {
        return "200px";
      }
    },
    // 千位符
    numberFormat(sums) {
      if (sums === 0) {
        sums = "";
      } else if (!isNaN(sums)) {
        if (sums % 1 === 0) {
          sums = `${sums}.00`;
        } else {
          sums = sums.toString();
        }
        let source = sums.split(".");
        source[0] = source[0].replace(
          new RegExp(/(\d)(?=(?:\d{3})+$)/g),
          "$1,"
        );
        sums = source.join(".");
      }
      return sums;
    },
    printBtn() {
      printExport("/expense/export", "print-pdf", "", this.querySchemer);
    },
    exportBtn() {
      printExport("/expense/export", "excel", "", this.querySchemer);
    },

    // 选中月份
    clickMoth(item) {
      console.log(item , this.startYear ,this.endYear);
      if(item >= this.startYear && item <= this.endYear) {
        this.active = item.fullyear;
        this.$emit("conDatePicker", item.fullyear);
      }
    },
    // 上一页
    pageUp() {
      if( this.yearArray[0] <= this.startYear ) {
        return false;
      }else {
        let startYear = this.yearArray[0]
        let newyYearArray = [];
        for (let i = 10; i >= 0; i--) {
          this.newyYearArray.push(startYear - i)
        }
        this.yearArray = newyYearArray
      }
    },
    // 下一页
    pageDown() {
      if( this.yearArray[this.yearArray.length - 1] >= this.endYear) {
        return false;
      }else {
        let endYear = this.yearArray[this.yearArray.length - 1]
        let newyYearArray = [];
        for (let i = 11; i < 11; i++) {
          this.newyYearArray.push(endYear + i)
        }
        this.yearArray = newyYearArray
      }
    }
  }
};
</script>
<style lang="less" scoped>
@import "../../../../style/base.less";

.main-top {
  .dateChange {
    float: left;
  }
  .btn-group {
    float: right;
    margin-right: 15px;
    button + button {
      margin-left: 10px;
    }
  }
}
.main-content.customer-box {
  .table-main {
    margin-bottom: 0;
  }
  .editPic {
    display: none;
    position: absolute;
    right: 10px;
    bottom: 6px;
  }
}
</style>
<style lang="less">
.profitForm {
  .el-table__body tr:hover,
  .el-table__body tr:active > td {
    background: #eee;
    .editPic {
      display: block;
    }
  }
  .el-table__body {
    // width: auto !important;
  }
}
.redColor {
  color: #f00;
}
.el-table .highline-row {
  background: #ecfeff;
}
</style>

<style lang="less" scoped>
.date-selection {
  width: 100px;
  padding-right: 10px;
  font-size: 14px;
  font-weight: bold;
  .pack-up {
    cursor: pointer;
  }
  .pack-up-defaut {
    cursor: pointer;
    color: #ddd;
  }
  .moth-title {
    font-size: 16px;
    margin: 10px 0;
    text-align: center;
    cursor: pointer;
  }
  .item-moth {
    li {
      line-height: 35px;
      cursor: pointer;
      text-align: center;
    }
    .active-item {
      color: #5478fb;
    }
  }
  .disable-text {
    color: #ddd;
  }
  .common-text {
    color: black;
  }
}
</style>
<style>
.date-selection .el-date-editor.el-input {
  width: 60px;
}
.date-selection .el-input--prefix .el-input__inner {
  padding-left: 0;
  padding-right: 0;
}
.date-selection .el-input__prefix {
  display: none;
}
.date-selection .el-tag {
  padding: 0;
}
</style>

```





### 费用统计表

```js
<template>
  <div class="main-subject profitForm">
    <div class="main-top">
      <div class="dateChange">
        <el-date-picker
          v-if="monthDateShow"
          :picker-options="disabledDate"
          style="width:170px; padding-right: 0;float: left"
          :editable="false"
          type="year"
          format="yyyy年"
          value-format="yyyy"
          :clearable="false"
          placeholder="选择期间"
          v-model="monthReportDate"
          @change="changeMonthReportDate"
        >
        </el-date-picker>
        <em class="el-icon-refresh-salf" circle @click="refreshList"></em>
      </div>
      <div class="btn-group">
        <el-button class="btn btn-main btn-import" @click="changeDetailsFlag"
          >{{ detailsTitle }}</el-button
        >
        <el-button class="btn btn-main btn-import" @click="printBtn"
          >打印</el-button
        >
        <el-button class="btn btn-main btn-import" @click="exportBtn"
          >导出</el-button
        >
      </div>
    </div>
    <div class="main-content customer-box">
      <el-col :span="2">
          <DateYearChoose
            ref="dateChoose"
            :date="timeInterval"
            :current-date="monthReportDate"
            @conDatePicker="selectDateGetList"
          />
        </el-col>
      <el-col :span="22">
      <div class="table-main" style="height: 100%">
        <el-table
          ref="mainTable"
          class="periodTable"
          :data="filterTableDate"
          border
          highlight-current-row
          height="98%"
          emptyText="暂无数据"
          :row-class-name="tableRowClassName"
        >
          <el-table-column :resizable="false" min-width="200px" align="left" label="项目">
            <template slot-scope="scope">
              <div>
                <span
                  :style="{
                    marginLeft: marginFn(scope.row),
                    fontWeight: scope.row.indentLevel === 0 ? 'bold' : 'normal'
                  }"
                  >{{ scope.row.expenseItemName }}</span
                >
              </div>
            </template>
          </el-table-column>
          <el-table-column min-width="200px" align="right" prop="janAmount" label="1月" >
            <template slot-scope="scope">
                <span>{{ numberFormat(scope.row.janAmount) }}</span>
            </template>
          </el-table-column>
          <el-table-column min-width="200px" align="right" prop="febAmount" label="2月" >
            <template slot-scope="scope">
                <span>{{ numberFormat(scope.row.febAmount) }}</span>
            </template>
          </el-table-column>
          <el-table-column min-width="200px" align="right" prop="marAmount" label="3月" >
            <template slot-scope="scope">
                <span>{{ numberFormat(scope.row.marAmount)  }}</span>
            </template>
          </el-table-column>
          <el-table-column min-width="200px" align="right" prop="aprAmount" label="4月" >
            <template slot-scope="scope">
                <span>{{ numberFormat(scope.row.aprAmount) }}</span>
            </template>
          </el-table-column>
          <el-table-column min-width="200px" align="right" prop="mayAmount" label="5月" >
            <template slot-scope="scope">
                <span>{{ numberFormat(scope.row.mayAmount) }}</span>
            </template>
          </el-table-column>
          <el-table-column min-width="200px" align="right" prop="junAmount" label="6月" >
            <template slot-scope="scope">
                <span>{{ numberFormat(scope.row.junAmount) }}</span>
            </template>
          </el-table-column>
          <el-table-column min-width="200px" align="right" prop="julAmount" label="7月" >
            <template slot-scope="scope">
                <span>{{ numberFormat(scope.row.julAmount) }}</span>
            </template>
          </el-table-column>
          <el-table-column min-width="200px" align="right" prop="augAmount" label="8月" >
            <template slot-scope="scope">
                <span>{{ numberFormat(scope.row.augAmount) }}</span>
            </template>
          </el-table-column>
          <el-table-column min-width="200px" align="right" prop="sepAmount" label="9月" >
            <template slot-scope="scope">
                <span>{{ numberFormat(scope.row.sepAmount) }}</span>
            </template>
          </el-table-column>
          <el-table-column min-width="200px" align="right" prop="octAmount" label="10月" >
            <template slot-scope="scope">
                <span>{{ numberFormat(scope.row.octAmount) }}</span>
            </template>
          </el-table-column>
          <el-table-column min-width="200px" align="right" prop="novAmount" label="11月" >
            <template slot-scope="scope">
                <span>{{ numberFormat(scope.row.novAmount) }}</span>
            </template>
          </el-table-column>
          <el-table-column min-width="200px" align="right" prop="decAmount" label="12月" >
            <template slot-scope="scope">
                <span>{{ numberFormat(scope.row.decAmount) }}</span>
            </template>
          </el-table-column>
          <el-table-column min-width="200px" align="right" prop="yearAmountStr" label="合计" >
            <template slot-scope="scope">
                <span>{{ scope.row.yearAmountStr }}</span>
            </template>
          </el-table-column>
        </el-table>
      </div>
    </el-col>

    </div>
  </div>
</template>

<script>
import StatementAjax from "@/api/statement/statement";
import { printExport } from "@/api/axios.config";
import { mapGetters } from "vuex";
import DateYearChoose from "@/components/dateSelection/dateYearChoose";
let page;
export default {
  props: {
    refreshPage: {
      type: String,
      default: ""
    }
  },
  components: {
    DateYearChoose
  },
  data() {
    return {
      monthDateShow: true,
      Period: "", // 当前会计期间
      startPeriod: "", // 启用期间
      end: "", // 凭证记账最大日期
      tableDate: null,
      filterTableDate: null,
      disabledDate: {
        disabledDate: time => {
          return (
            new Date(page.startPeriod).getTime() >= time.getTime() ||
            new Date(page.end).getTime() <= time.getTime()
          );
        }
      },
      monthReportDate: "",
      querySchemer: {
        date: ""
      },
      detailsTitle: '显示费用明细',
      detailFlag: true,
      timeInterval: {
        start:null,
        end:null,
      },
      formTiem: {},
    };
  },
  created() {
    page = this;
    this.init();
  },
  watch: {
    $route(to, from) {
      if (from.name === "checkoutTest" && to.query.currentPeriod) {
        this.monthReportDate = to.query.currentPeriod;
        this.AssetsDebtReportSearchForm.period = this.monthReportDate;
        // this.assetsDebtList();
      }
    },
    refreshPage(newVal, oldVal) {
      if (newVal === "profitForm") {
        this.queryPeriod("clickTab");
      }
    },
    deep: true
  },
  computed: {
    ...mapGetters(["getTodoById", "getOrder"])
  },
  methods: {
    init() {
      init.bind(this)();
      async function init() {
        console.log(this);
        await this.queryPeriod();
      }
    },
    queryProfitList(date) {
      // 查询费用统计表
      console.log('重制查询费用统计表',date);
      StatementAjax.ExpenseList(date).then(res => {
        if (res.code === 200) {
          // 发送请求
          if (res.code == 200) {
            this.tableDate = res.data;
            this.filterTableDate = res.data;
          }
        }
      });
    },
    async queryPeriod(fn) {
      // 会计期间
      return new Promise((resolve, reject) => {
        StatementAjax.QueryAccountPeriod().then(res => {
          if (res.code === 200) {
            this.Period = res.data.currentPeriod;
            this.monthReportDate = res.data.currentPeriod;
            this.startPeriod = res.data.start;
            this.end = res.data.end;
            this.timeInterval.start = res.data.start;
            this.timeInterval.end = res.data.end;
            this.$refs.dateChoose.getDate(this.timeInterval)
            this.querySchemer.date = res.data.currentPeriod.split("-")[0];
            resolve(res.data);
            let year = new Date(this.Period).getFullYear();
            year = `date=${year}`
            this.queryProfitList(year);
          }
        });
      });
    },
    refreshList() {
      let year = new Date(this.monthReportDate).getFullYear();
      year = `date=${year}`
      this.queryProfitList(year);
    },
    //左侧年份
    selectDateGetList(val) {
      this.monthReportDate = val + "-01-01";
      let date = `date=${val}`
      // 搜索
      this.queryProfitList(date)
    },
    changeDetailsFlag() {
      this.detailsFlag = !this.detailsFlag
      this.detailsTitle =  this.detailsFlag ? '显示费用明细' : '隐藏费用明细'
      if(this.detailsFlag) {
        this.filterTableDate = this.tableDate.filter((item) => item.indentLevel == 0)
      }else {
        this.filterTableDate = this.tableDate
      }
    },
    tableRowClassName({row, rowIndex}) {
        if (row.indentLevel == 0) {
          return 'highline-row';
        }
        return '';
    },
    changeMonthReportDate(val) {
      this.monthReportDate = val+'-01-01';
      if (val === null) return;
      this.querySchemer.date = val;
      this.Period = val;
      this.$refs.dateChoose.setActiveYear(val)
      let newVal = `date=${val}`;
      this.queryProfitList(newVal);
    },
    marginFn(val) {
      if (val.indentLevel === 1) {
        return "20px";
      }
      if (val.indentLevel === 2) {
        return "40px";
      }
      if (val.indentLevel === 3) {
        return "60px";
      }
      if (val.indentLevel === 4) {
        return "80px";
      }
      if (val.indentLevel === 5) {
        return "200px";
      }
    },
    // 千位符
    numberFormat(sums) {
      if (sums === 0) {
        sums = "";
      } else if (!isNaN(sums)) {
        if (sums % 1 === 0) {
          sums = `${sums}.00`;
        } else {
          sums = sums.toString();
        }
        let source = sums.split(".");
        source[0] = source[0].replace(
          new RegExp(/(\d)(?=(?:\d{3})+$)/g),
          "$1,"
        );
        sums = source.join(".");
      }
      return sums;
    },
    printBtn() {
      printExport("/expense/export", "print-pdf", "", this.querySchemer);
    },
    exportBtn() {
      printExport("/expense/export", "excel", "", this.querySchemer);
    }
  }
};
</script>
<style lang="less" scoped>
@import "../../../../style/base.less";

.main-top {
  .dateChange {
    float: left;
  }
  .btn-group {
    float: right;
    margin-right: 15px;
    button + button {
      margin-left: 10px;
    }
  }
}
.main-content.customer-box {
  .table-main {
    margin-bottom: 0;
  }
  .editPic {
    display: none;
    position: absolute;
    right: 10px;
    bottom: 6px;
  }
}
</style>
<style lang="less">
.profitForm {
  .el-table__body tr:hover,
  .el-table__body tr:active > td {
    background: #eee;
    .editPic {
      display: block;
    }
  }
  .el-table__body {
    // width: auto !important;
  }
}
.redColor {
  color: #f00;
}
.el-table .highline-row {
  background: #ecfeff;
}
</style>

```

### 年份时间组件

```js
<template>
  <div class="date-selection">
    <p class="moth-title" @click="pageUp">
      <!-- {{ activeYear }} -->
      <i
        :class="
          yearArrs[0] <= startYear
            ? 'pack-up-defaut el-icon-arrow-up'
            : 'pack-up el-icon-arrow-up'
        "
      />
    </p>
    <div class="item-moth">
      <ul>
        <li
          v-for="(item, index) in yearArr"
          :class="[
            item.disabled ? 'disable-text' : '',
            active == `${item.year}-01-01` ? 'active-item' : ''
          ]"
          @click="item.disabled ? null : clickYear(item)"
          :key="item.year"
        >
          <p>{{ item.year }}年</p>
        </li>
      </ul>
    </div>
    <!-- <p>
      <i
        @click="pageDown"
        style="font-size: 24px;"
        :class="Number(activeYear)>=endYear?'pack-up-defaut el-icon-arrow-down':'pack-up el-icon-arrow-down'"
      ></i>
    </p> -->
    <p class="moth-title" @click="pageDown">
      <!-- {{ Number(activeYear) + 1 }} -->
      <i
        :class="
          yearArrs[9] >= endYear
            ? 'pack-up-defaut el-icon-arrow-down'
            : 'pack-up el-icon-arrow-down'
        "
      />
    </p>
  </div>
</template>

<script>
import { mapGetters } from "vuex";

export default {
  props: {
    date: {
      type: Object,
      default: Object.create(null)
    },
    source: {
      type: String,
      default: ""
    },
    currentDate: {
      type: String,
      default: ""
    }
  },

  data() {
    return {
      mothArray: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
      startYear: 0,
      endYear: 0,
      startMoth: "",
      endMoth: "",
      activeYear: 0,
      active: null,
      selectDateYear: null,
      currentPeriodYear: null,
      currentPeriodMonth: null,
      startMothyear: null,
      datePicker: null,
      yearArrs: []
    };
  },
  computed: {
    ...mapGetters(["formatDate"]),
    dateRangeStamp() {
      let { start, end } = this.date;
      return {
        start: +new Date(start) || 0,
        end: +new Date(end) || 0
      };
    },
    // +++++++++++++++
    yearArr() {
      const { activeYear, dateRangeStamp } = this;
      const { start, end } = dateRangeStamp;
      let startYear = new Date(start).getFullYear();
      let endYear = new Date(end).getFullYear();
      return this.mothArray.map(it => {
        let year = parseInt(activeYear/10);
        year = year * 10 + it - 1;
        const disabled = year < startYear || year > endYear;
        return {
          disabled,
          year: year
        };
      });
    }
  },
  watch: {
    date: {
      immediate: true,
      handler: function(n) {
        if (n && n.start) {
          const start = new Date(n.start);
          let startYear = start.getFullYear();
          let startMonth = start.getMonth() + 1;
          startMonth = startMonth > 9 ? startMonth : `0${startMonth}`;
          let endYear = new Date(n.end).getFullYear();
          // this.activeYear = startYear;
          this.startYear = startYear;
          this.endYear = endYear;
          // this.active = `${startYear}-${startMonth}-01`;
        }
      }
    },
    currentDate: {
      immediate: true,
      handler: function(n) {
        if (n) {
          let currentArr = n.split('-')
          let year = currentArr[0]
          let month = currentArr[1]
          this.activeYear = year
          this.active = `${year}-${month}-01`;
        }
      }
    }
  },
  methods: {
    getDate(val) {
      this.startYear = new Date(val.start).getFullYear();
      this.endYear = new Date(val.end).getFullYear();
    },
    setActiveYear(val) {
      this.active = val+'-01-01';
    },
    // itemStyle(item) {
    //   let months = item;
    //   let startMoth =
    //     this.startMoth > 10 ? this.startMoth : this.startMoth.slice(1);

    //   let endMoth = this.endMoth > 9 ? this.endMoth : this.endMoth.slice(1);
    //   if (this.startMothyear == this.activeYear) {
    //     if (months < startMoth || months > endMoth) {
    //       return "disable-text";
    //     } else {
    //       return "";
    //     }
    //   } else {
    //     return "disable-text";
    //   }
    // },

    // 设置当前期间
    // setCurrentPeriod(timeInterval) {
    //   this.startMoth = timeInterval.start.slice(5, 7);
    //   this.startMothyear = timeInterval.start.slice(0, 4);
    //   this.endMoth = this.date.end.slice(5, 7);
    //   this.endMothyear = this.date.end.slice(0, 4);
    // },
    // 修改日期
    // handelChangeDate() {
    //   console.log(this.datePicker, "this.datePicker");
    //   this.$emit("getDate", this.datePicker);
    //   let datePickerArr = this.datePicker.slice(0, 7).split("-");
    //   this.active = Number(datePickerArr[1]);
    //   this.activeYear = datePickerArr[0];
    //   this.selectDateYear = this.activeYear;
    // },
    // 选中年
    clickYear(item) {
      this.active = item.year + '-01-01';
      this.$emit("conDatePicker", item.year);
    },
    // 上一页
    pageUp() {
      if(this.yearArr[0].year <= this.startYear) {
        return false
      }else {
        this.activeYear = Number(this.activeYear) - 10
      }
    },
    // 下一页
    pageDown() {
      if(this.yearArr[9].year < this.endYear) {
        return false
      }else {
        this.activeYear = Number(this.activeYear) + 10
      }
    }
  }
};
</script>

<style lang="less" scoped>
.date-selection {
  padding-right: 10px;
  font-size: 14px;
  font-weight: bold;
  .pack-up {
    cursor: pointer;
  }
  .pack-up-defaut {
    cursor: pointer;
    color: #ddd;
  }
  .moth-title {
    font-size: 16px;
    margin: 10px 0;
    text-align: center;
    cursor: pointer;
  }
  .item-moth {
    user-select: none;
    li {
      line-height: 35px;
      cursor: pointer;
      text-align: center;
    }
    .active-item {
      color: #5478fb;
    }
  }
  .disable-text {
    color: #ddd;
  }
  .common-text {
    color: black;
  }
}
</style>
<style>
.date-selection .el-date-editor.el-input {
  width: 60px;
}
.date-selection .el-input--prefix .el-input__inner {
  padding-left: 0;
  padding-right: 0;
}
.date-selection .el-input__prefix {
  display: none;
}
.date-selection .el-tag {
  padding: 0;
}
</style>

```

