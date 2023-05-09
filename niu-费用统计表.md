## 项目需求

### 1. 报表/费用统计表

```js
<template>
  <div style="height: 100%; display: flex; flex-direction: column">
    <e-c-header />
    <div class="main">
      <div class="search-section">
        <!-- <div class="search-wrapper">
             <div class="search-box">
             <div
                style="display: -webkit-box;
                        width: 84px;
                        line-height: 19px;
                        overflow: hidden;
                        text-overflow: ellipsis;
                        line-clamp: 2;
                        -webkit-line-clamp: 2;
                        word-wrap: break-word;
                        -webkit-box-orient: vertical;
                        font-size: 14px;
                        cursor: pointer;"
                :title="companyName"
                @click="accountDetail"
              >{{companyName}}</div>
               <div class="search-period">{{period}}</div>
              <div class="search-switch" id="toggleSearchModal" @click.stop="toggleSearchModal">切换</div>
              <div class="search-modal" v-if="searchModalShow" v-clickoutside="closeSearchModal">
                <div class="left-arrow"></div>
                <div style="width: 190px; height: 32px; margin-top: 11px;">
                  <input type="text" class="search-text" v-model="keyword" @input="queryCustomerList" placeholder="请输入搜索内容" />
                </div>
                <ul class="search-panel">
                  <li
                    class="search-item"
                    @click="selectCustomer(item)"
                    v-for="item in customerList"
                    :title="item.name"
                    :key="item.id"
                  >
                    {{item.name}}
                  </li>
                </ul>
              </div>
            </div>
          </div> -->
        <div class="left-menu">
          <el-menu
            default-active="onRouters"
            :unique-opened="true"
            @open="openSubmenu"
            @close="closeSubmenu"
            text-color="#ffffff"
            background-color="#2b314b"
            active-text-color="#ffffff"
            class="el-menu-vertical-demo"
            router
          >
            <!-- v-if="getTodoById('Vindex/add')" -->

            <div class="menuMain">
              <ul class="first-menu-level">
                <li class="menuList menu1">
                  <el-menu-item
                    @click="toHome()"
                    class="first-menu"
                    :class="{ isSelectLi: menuIndex === 'index' }"
                  >
                    <span class="menu-icon"></span>
                    首页
                  </el-menu-item>
                </li>
              </ul>
            </div>

            <!-- <div class="menuTitle">
                <span class="menuContent">清单</span>
              </div> -->
            <div class="menuMain">
              <ul class="first-menu-level">
                <li
                  class="menuList child-menu"
                  :class="{ childMenuBox: activeStatus['1-1'] }"
                  v-if="
                    getTodoById('invoice-income/list') ||
                    getTodoById('invoice-output/list') ||
                    getTodoById('invoice-cost/list')
                  "
                >
                  <el-submenu index="1-1">
                    <template slot="title">
                      <span
                        class="secondMenus"
                        :class="{
                          menu2:
                            menuIndex === 'incomeInvoice' ||
                            menuIndex === 'outputInvoice' ||
                            menuIndex === 'cost',
                        }"
                        ><i class="menu-icon invoice"></i>发票</span
                      >
                    </template>
                    <ul ref="invoiceBox">
                      <li
                        class="check-child"
                        v-if="getTodoById('invoice-income/list')"
                      >
                        <el-menu-item
                          index="/center/incomeInvoice"
                          @click="addTab('incomeInvoice')"
                          :class="{ isSelectLi: menuIndex === 'incomeInvoice' }"
                          >进项发票</el-menu-item
                        >
                      </li>
                      <li
                        class="check-child"
                        v-if="getTodoById('invoice-output/list')"
                      >
                        <el-menu-item
                          index="/center/outputInvoice"
                          @click="addTab('outputInvoice')"
                          :class="{ isSelectLi: menuIndex === 'outputInvoice' }"
                          >销项发票</el-menu-item
                        >
                      </li>
                      <li
                        class="check-child"
                        v-if="getTodoById('invoice-cost/list')"
                      >
                        <el-menu-item
                          index="/center/cost"
                          @click="addTab('cost')"
                          :class="{ isSelectLi: menuIndex === 'cost' }"
                          >费用</el-menu-item
                        >
                      </li>
                    </ul>
                  </el-submenu>
                </li>
                <li
                  class="menuList child-menu"
                  :class="{ childMenuBox: activeStatus['1-2'] }"
                  v-if="
                    getTodoById('fund-bank/list') ||
                    getTodoById('fund-cash/list') ||
                    getTodoById('fund-other-cash/list')
                  "
                >
                  <el-submenu index="1-2">
                    <template slot="title">
                      <span
                        class="secondMenus"
                        :class="{
                          menu2:
                            menuIndex === 'invoiceBank' ||
                            menuIndex === 'cash' ||
                            menuIndex === 'otherFund',
                        }"
                        ><i class="menu-icon fund"></i>资金</span
                      >
                    </template>
                    <ul ref="fundBox">
                      <li
                        class="check-child"
                        v-if="getTodoById('fund-bank/list')"
                      >
                        <el-menu-item
                          index="/center/invoiceBank"
                          @click="addTab('invoiceBank')"
                          :class="{ isSelectLi: menuIndex === 'invoiceBank' }"
                          >银行</el-menu-item
                        >
                      </li>
                      <li
                        class="check-child"
                        v-if="getTodoById('fund-cash/list')"
                      >
                        <el-menu-item
                          index="/center/cash"
                          @click="addTab('cash')"
                          :class="{ isSelectLi: menuIndex === 'cash' }"
                          >现金</el-menu-item
                        >
                      </li>
                      <li
                        class="check-child"
                        v-if="getTodoById('fund-other-cash/list')"
                      >
                        <el-menu-item
                          index="/center/otherFund"
                          @click="addTab('otherFund')"
                          :class="{ isSelectLi: menuIndex === 'otherFund' }"
                          >其他货币资金</el-menu-item
                        >
                      </li>
                    </ul>
                  </el-submenu>
                </li>
                <li
                  class="menuList child-menu"
                  :class="{ childMenuBox: activeStatus['1-3'] }"
                  v-if="
                    getTodoById('fixed-assets/list') ||
                    getTodoById('intangible-assets/list') ||
                    getTodoById('un-amortized-expense-assets/list') ||
                    getTodoById('amortized-depreciate-summary/list') ||
                    getTodoById('amortized-depreciate-summary/list') ||
                    getTodoById('assets-clear/list')
                  "
                >
                  <el-submenu index="1-3">
                    <template slot="title">
                      <span
                        class="secondMenus"
                        :class="{
                          menu2:
                            menuIndex === 'fixedProperty' ||
                            menuIndex === 'invisibleProperty' ||
                            menuIndex === 'deferredAssets' ||
                            menuIndex === 'detailAmortOfDepre' ||
                            menuIndex === 'summaryAmortOfDepre' ||
                            menuIndex === 'cleanProperty',
                        }"
                        ><i class="menu-icon property"></i>资产</span
                      >
                    </template>
                    <ul ref="assetsBox">
                      <li
                        class="check-child"
                        v-if="getTodoById('fixed-assets/list')"
                      >
                        <el-menu-item
                          index="/center/fixedProperty"
                          @click="addTab('fixedProperty')"
                          :class="{ isSelectLi: menuIndex === 'fixedProperty' }"
                          >固定资产</el-menu-item
                        >
                      </li>
                      <li
                        class="check-child"
                        v-if="getTodoById('intangible-assets/list')"
                      >
                        <el-menu-item
                          index="/center/invisibleProperty"
                          @click="addTab('invisibleProperty')"
                          :class="{
                            isSelectLi: menuIndex === 'invisibleProperty',
                          }"
                          >无形资产</el-menu-item
                        >
                      </li>
                      <li
                        class="check-child"
                        v-if="getTodoById('un-amortized-expense-assets/list')"
                      >
                        <el-menu-item
                          index="/center/deferredAssets"
                          @click="addTab('deferredAssets')"
                          :class="{
                            isSelectLi: menuIndex === 'deferredAssets',
                          }"
                          >长期待摊费用</el-menu-item
                        >
                      </li>
                      <li
                        class="check-child"
                        v-if="getTodoById('amortized-depreciate-details/list')"
                      >
                        <el-menu-item
                          index="/center/detailAmortOfDepre"
                          @click="addTab('detailAmortOfDepre')"
                          :class="{
                            isSelectLi: menuIndex === 'detailAmortOfDepre',
                          }"
                          >折旧摊销明细</el-menu-item
                        >
                      </li>
                      <li
                        class="check-child"
                        v-if="getTodoById('amortized-depreciate-summary/list')"
                      >
                        <el-menu-item
                          index="/center/summaryAmortOfDepre"
                          @click="addTab('summaryAmortOfDepre')"
                          :class="{
                            isSelectLi: menuIndex === 'summaryAmortOfDepre',
                          }"
                          >折旧摊销汇总</el-menu-item
                        >
                      </li>
                      <li
                        class="check-child"
                        v-if="getTodoById('assets-clear/list')"
                      >
                        <el-menu-item
                          index="/center/cleanProperty"
                          @click="addTab('cleanProperty')"
                          :class="{ isSelectLi: menuIndex === 'cleanProperty' }"
                          >资产清理处置</el-menu-item
                        >
                      </li>
                    </ul>
                  </el-submenu>
                </li>
              </ul>
            </div>
            <!-- <div class="split-line"></div> -->
            <!-- <div class="menuTitle">
                <span class="menuContent">记账</span>
              </div> -->
            <div class="menuMain">
              <ul class="first-menu-level">
                <li
                  class="menuList menu1"
                  v-if="getTodoById('voucher-entry/add')"
                >
                  <el-menu-item
                    index="/center/voucherInput"
                    @click="addTab('voucherInput')"
                    class="first-menu"
                    :class="{ isSelectLi: menuIndex === 'voucherInput' }"
                    ><span class="menu-icon voucher-input"></span
                    >凭证录入</el-menu-item
                  >
                </li>
                <!-- <li
                  class="menuList menu1"
                  v-if="getTodoById('voucher-entry/add')"
                >
                  <el-menu-item
                    index="/center/voucherInputN"
                    @click="addTab('voucherInputN')"
                    class="first-menu"
                    :class="{ isSelectLi: menuIndex === 'voucherInputN' }"
                    ><span class="menu-icon voucher-input"></span
                    >凭证录入N</el-menu-item
                  >
                </li> -->
                <li class="menuList menu1">
                  <el-menu-item
                    index="/center/voucherInfo"
                    @click="addTab('voucherInfo')"
                    :class="{ isSelectLi: menuIndex === 'voucherInfo' }"
                    v-if="false"
                    ><i class=""></i>凭证信息</el-menu-item
                  >
                </li>
                <li
                  class="menuList menu1"
                  v-if="getTodoById('voucher-query/list')"
                >
                  <el-menu-item
                    index="/center/voucherQuery"
                    @click="addTab('voucherQuery')"
                    class="first-menu"
                    :class="{ isSelectLi: menuIndex === 'voucherQuery' }"
                    ><span class="menu-icon voucher-detail"></span
                    >凭证查询</el-menu-item
                  >
                </li>
                <li
                  class="menuList child-menu"
                  :class="{ childMenuBox: activeStatus['2-1'] }"
                  v-if="
                    getTodoById('account-book/general-ledger') ||
                    getTodoById('account-book/subsidiary-ledger') ||
                    getTodoById('account-book/account-balance-sheet') ||
                    getTodoById('account-book/account-summary')
                  "
                >
                  <el-submenu index="2-1">
                    <template slot="title">
                      <span
                        class="secondMenus"
                        :class="{
                          menu2:
                            menuIndex === 'ledger' ||
                            menuIndex === 'detailAccount' ||
                            menuIndex === 'chronological' ||
                            menuIndex === 'subjectBalance' ||
                            menuIndex === 'summaryOfSubjects' ||
                            menuIndex === 'detailAccountQuantity' ||
                            menuIndex === 'subjectBalanceQuantity' ||
                            menuIndex === 'detailAccountAuxiliary' ||
                            menuIndex === 'subjectBalanceAuxiliary',
                        }"
                        ><i class="menu-icon account-book"></i>账簿</span
                      >
                    </template>
                    <ul ref="accountBox">
                      <li
                        class="check-child"
                        v-if="getTodoById('account-book/general-ledger')"
                      >
                        <el-menu-item
                          index="/center/ledger"
                          @click="addTab('ledger')"
                          :class="{ isSelectLi: menuIndex === 'ledger' }"
                          >总账</el-menu-item
                        >
                      </li>
                      <li
                        class="check-child"
                        v-if="getTodoById('account-book/subsidiary-ledger')"
                      >
                        <el-menu-item
                          index="/center/detailAccount"
                          @click="addTab('detailAccount')"
                          :class="{ isSelectLi: menuIndex === 'detailAccount' }"
                          >明细账</el-menu-item
                        >
                      </li>
                      <li
                        class="check-child"
                        v-if="getTodoById('account-book/account-chronological')"
                      >
                        <el-menu-item
                          index="/center/chronological"
                          @click="addTab('chronological')"
                          :class="{ isSelectLi: menuIndex === 'chronological' }"
                          >序时账</el-menu-item
                        >
                      </li>
                      <li
                        class="check-child"
                        v-if="getTodoById('account-book/account-balance-sheet')"
                      >
                        <el-menu-item
                          index="/center/subjectBalance"
                          @click="addTab('subjectBalance')"
                          :class="{
                            isSelectLi: menuIndex === 'subjectBalance',
                          }"
                          >科目余额表</el-menu-item
                        >
                      </li>
                      <li
                        class="check-child"
                        v-if="getTodoById('account-book/account-summary')"
                      >
                        <el-menu-item
                          index="/center/summaryOfSubjects"
                          @click="addTab('summaryOfSubjects')"
                          :class="{
                            isSelectLi: menuIndex === 'summaryOfSubjects',
                          }"
                          >科目汇总表</el-menu-item
                        >
                      </li>
                      <li
                        class="check-child"
                        v-if="
                          getTodoById('account-book/quantity-subsidiary-ledger')
                        "
                      >
                        <el-menu-item
                          index="/center/detailAccountQuantity"
                          @click="addTab('detailAccountQuantity')"
                          :class="{
                            isSelectLi: menuIndex === 'detailAccountQuantity',
                          }"
                          >数量核算明细账</el-menu-item
                        >
                      </li>
                      <li
                        class="check-child"
                        v-if="
                          getTodoById('account-book/quantity-balance-sheet')
                        "
                      >
                        <el-menu-item
                          index="/center/subjectBalanceQuantity"
                          @click="addTab('subjectBalanceQuantity')"
                          :class="{
                            isSelectLi: menuIndex === 'subjectBalanceQuantity',
                          }"
                          >数量核算余额表</el-menu-item
                        >
                      </li>
                      <li
                        class="check-child"
                        v-if="
                          getTodoById(
                            'account-book/auxiliary-subsidiary-ledger'
                          )
                        "
                      >
                        <el-menu-item
                          index="/center/detailAccountAuxiliary"
                          @click="addTab('detailAccountAuxiliary')"
                          :class="{
                            isSelectLi: menuIndex === 'detailAccountAuxiliary',
                          }"
                          >辅助核算明细账</el-menu-item
                        >
                      </li>
                      <li
                        class="check-child"
                        v-if="
                          getTodoById('account-book/auxiliary-balance-sheet')
                        "
                      >
                        <el-menu-item
                          index="/center/subjectBalanceAuxiliary"
                          @click="addTab('subjectBalanceAuxiliary')"
                          :class="{
                            isSelectLi: menuIndex === 'subjectBalanceAuxiliary',
                          }"
                          >辅助核算余额表</el-menu-item
                        >
                      </li>
                    </ul>
                  </el-submenu>
                </li>
                <li
                  class="menuList child-menu"
                  :class="{ childMenuBox: activeStatus['2-2'] }"
                  v-if="
                    getTodoById('profit/list') ||
                    getTodoById('cash-flow/list') ||
                    getTodoById('assets-debt/list')
                  "
                >
                  <el-submenu index="2-2">
                    <template slot="title">
                      <span
                        class="secondMenus"
                        :class="{
                          menu2:
                            menuIndex === 'profitForm' ||
                            menuIndex === 'cashFlowForm' ||
                            menuIndex === 'balanceSheet' ||
                            menuIndex === 'expenseStatistics',
                        }"
                        ><i class="menu-icon statement"></i>报表</span
                      >
                    </template>
                    <ul ref="reportFormBox">
                      <li class="check-child" v-if="getTodoById('profit/list')">
                        <el-menu-item
                          index="/center/profitForm"
                          @click="addTab('profitForm')"
                          :class="{ isSelectLi: menuIndex === 'profitForm' }"
                          >利润表</el-menu-item
                        >
                      </li>
                      <li class="check-child" v-if="getTodoById('profit/list-quarter')">
                        <el-menu-item
                          index="/center/profitFormQuarterlyReport"
                          @click="addTab('profitFormQuarterlyReport')"
                          :class="{ isSelectLi: menuIndex === 'profitFormQuarterlyReport' }"
                          >利润表季报</el-menu-item
                        >
                      </li>
                      <li
                        class="check-child"
                        v-if="getTodoById('cash-flow/list')"
                      >
                        <el-menu-item
                          index="/center/cashFlowForm"
                          @click="addTab('cashFlowForm')"
                          :class="{ isSelectLi: menuIndex === 'cashFlowForm' }"
                          >现金流量表</el-menu-item
                        >
                      </li>
                      <li
                        class="check-child"
                        v-if="getTodoById('cash-flow/list-quarter')"
                      >
                        <el-menu-item
                          index="/center/cashFlowFormQuarterlyReport"
                          @click="addTab('cashFlowFormQuarterlyReport')"
                          :class="{ isSelectLi: menuIndex === 'cashFlowFormQuarterlyReport' }"
                          >现金流量表季报</el-menu-item
                        >
                      </li>
                      <li
                        class="check-child"
                        v-if="getTodoById('assets-debt/list')"
                      >
                        <el-menu-item
                          index="/center/balanceSheet"
                          @click="addTab('balanceSheet')"
                          :class="{ isSelectLi: menuIndex === 'balanceSheet' }"
                          >资产负债表</el-menu-item
                        >
                      </li>

                      <!-- <li
                        class="check-child"
                        v-if="getTodoById('assets-debt/list1')"
                      >
                        <el-menu-item
                          index="/center/expenseStatistics"
                          @click="addTab('expenseStatistics')"
                          :class="{ isSelectLi: menuIndex === 'expenseStatistics' }"
                          >费用统计表</el-menu-item
                        >
                      </li> -->

                    </ul>
                  </el-submenu>
                </li>
                <li
                  class="menuList menu1"
                  v-if="getTodoById('checkout-checkout/list')"
                >
                  <el-menu-item
                    index="/center/checkoutTest"
                    @click="addTab('checkoutTest')"
                    class="first-menu"
                    :class="{ isSelectLi: menuIndex === 'checkoutTest' }"
                    ><span class="menu-icon total-account"></span
                    >结账</el-menu-item
                  >
                </li>
                <li
                    class="menuList child-menu"
                    :class="{ childMenuBox: activeStatus['2-3'] }"
                    v-if="
                      getTodoById('personalTax-personalTax/personalTax') ||
                      getTodoById('addedTax-addedTax/addedTax') 
                    "
                  >
                <el-submenu index="2-3">
                  <template slot="title">
                        <span
                          class="secondMenus"
                          :class="{
                            menu2:
                              menuIndex === 'personalTax' ||
                              menuIndex === 'addedTax'
                          }"
                          ><i class="menu-icon smart-tax"></i>智能税务</span
                        >
                      </template>
                    <ul ref="personalTaxBox">
                    <li
                      class="check-child"
                      v-if="getTodoById('personalTax-personalTax/personalTax')"
                    >
                      <el-menu-item
                        index="/center/personalTax/personalTax"
                        @click="addTab('personalTax')"
                        :class="{ isSelectLi: menuIndex === 'personalTax' }"
                        >企业所得税</el-menu-item
                      >
                    </li>
                    <li
                      class="check-child"
                      v-if="getTodoById('addedTax-addedTax/addedTax')"
                    >
                      <el-menu-item
                        index="/center/personalTax/addedTax"
                        @click="addTab('addedTax')"
                        :class="{ isSelectLi: menuIndex === 'addedTax' }"
                        >增值税及附加税</el-menu-item
                      >
                    </li>
                  </ul>
                </el-submenu>
              </li>
          </ul>
        </div>
            <!-- <div class="split-line"></div> -->
            <!-- <div class="menuTitle">
                <span class="menuContent">设置</span>
              </div> -->
            <div class="menuMain">
              <ul class="first-menu-level">
                <li class="menuList menu1">
                  <el-menu-item
                    index="/center/index"
                    @click="addTab('index')"
                    :class="{ isSelectLi: menuIndex === 'index' }"
                    v-if="false"
                    >首页</el-menu-item
                  >
                </li>
                <li class="menuList menu1" v-if="getTodoById('accounts/list')">
                  <el-menu-item
                    index="/center/subject"
                    @click="addTab('subject')"
                    class="first-menu"
                    :class="{ isSelectLi: menuIndex === 'subject' }"
                    ><span class="menu-icon subject"></span>科目</el-menu-item
                  >
                </li>
                <li class="menuList menu1" v-if="getTodoById('begins/list')">
                  <el-menu-item
                    index="/center/earlyStage"
                    @click="addTab('earlyStage')"
                    class="first-menu"
                    :class="{ isSelectLi: menuIndex === 'earlyStage' }"
                    ><span class="menu-icon early-stage"></span
                    >期初</el-menu-item
                  >
                </li>
                <li
                  class="menuList child-menu"
                  :class="{ childMenuBox: activeStatus['3-1'] }"
                  v-if="
                    getTodoById('assist/list-customer') ||
                    getTodoById('assist/list-provider') ||
                    getTodoById('assist/list-employee') ||
                    getTodoById('assist/list-department') ||
                    getTodoById('assist/list-project') ||
                    getTodoById('assist/list-stock')
                  "
                >
                  <el-submenu index="3-1">
                    <template slot="title">
                      <span
                        class="secondMenus"
                        :class="{
                          menu2:
                            menuIndex === 'customer' ||
                            menuIndex === 'supplier' ||
                            menuIndex === 'staff' ||
                            menuIndex === 'department' ||
                            menuIndex === 'project' ||
                            menuIndex === 'stock',
                        }"
                        ><i class="menu-icon assist-account"></i>辅助核算</span
                      >
                    </template>
                    <ul ref="assistBox">
                      <li
                        class="check-child"
                        v-if="getTodoById('assist/list-customer')"
                      >
                        <el-menu-item
                          index="/center/customer"
                          @click="addTab('customer')"
                          :class="{ isSelectLi: menuIndex === 'customer' }"
                          >客户</el-menu-item
                        >
                      </li>
                      <li
                        class="check-child"
                        v-if="getTodoById('assist/list-provider')"
                      >
                        <el-menu-item
                          index="/center/supplier"
                          @click="addTab('supplier')"
                          :class="{ isSelectLi: menuIndex === 'supplier' }"
                          >供应商</el-menu-item
                        >
                      </li>
                      <li
                        class="check-child"
                        v-if="getTodoById('assist/list-employee')"
                      >
                        <el-menu-item
                          index="/center/staff"
                          @click="addTab('staff')"
                          :class="{ isSelectLi: menuIndex === 'staff' }"
                          >员工</el-menu-item
                        >
                      </li>
                      <li
                        class="check-child"
                        v-if="getTodoById('assist/list-department')"
                      >
                        <el-menu-item
                          index="/center/department"
                          @click="addTab('department')"
                          :class="{ isSelectLi: menuIndex === 'department' }"
                          >部门</el-menu-item
                        >
                      </li>
                      <li
                        class="check-child"
                        v-if="getTodoById('assist/list-project')"
                      >
                        <el-menu-item
                          index="/center/project"
                          @click="addTab('project')"
                          :class="{ isSelectLi: menuIndex === 'project' }"
                          >项目</el-menu-item
                        >
                      </li>
                      <li
                        class="check-child"
                        v-if="getTodoById('assist/list-stock')"
                      >
                        <el-menu-item
                          index="/center/commodityType"
                          @click="addTab('commodityType')"
                          :class="{ isSelectLi: menuIndex === 'commodityType' }"
                          >存货及服务</el-menu-item
                        >
                      </li>
                    </ul>
                  </el-submenu>
                </li>
                <li class="menuList menu1">
                  <el-menu-item
                    index="/center/actionLog"
                    @click="addTab('actionLog')"
                    class="first-menu"
                    :class="{ isSelectLi: menuIndex === 'actionLog' }"
                    ><span class="menu-icon action-log"></span
                    >操作日志</el-menu-item
                  >
                </li>
                <li
                  class="menuList menu1"
                  v-if="getTodoById('voucher-import/import')"
                >
                  <el-menu-item
                    class="first-menu"
                    index="/center/voucherExport"
                    @click="addTab('voucherExport')"
                    :class="{ isSelectLi: menuIndex === 'voucherExport' }"
                    ><span class="menu-icon voucher-export"></span
                    >凭证导入</el-menu-item
                  >
                </li>
                <li class="menuList menu1" v-if="getTodoById('bills/list')">
                  <el-menu-item
                    index="/center/itemType"
                    @click="addTab('itemType')"
                    class="first-menu"
                    :class="{ isSelectLi: menuIndex === 'itemType' }"
                    ><span class="menu-icon item-type"></span
                    >单据类型设置</el-menu-item
                  >
                </li>
                <!-- <li class="menuList menu1">
                    <el-menu-item index="/center/commodityType" @click="addTab('commodityType')" class="first-menu" :class="{isSelectLi:menuIndex==='commodityType'}"><span class="menu-icon fund-code"></span>商品编码设置</el-menu-item>
                  </li> -->
                <li
                  class="menuList menu1"
                  v-if="getTodoById('bank-state/list')"
                >
                  <el-menu-item
                    class="first-menu"
                    index="/center/bank"
                    @click="addTab('bank')"
                    :class="{ isSelectLi: menuIndex === 'bank' }"
                    ><span class="menu-icon bank-icon"></span>银行</el-menu-item
                  >
                </li>
              </ul>
            </div>
            <!-- <div class="split-line"></div> -->
            <!-- <span style="display: block;height: 10px;width: 100%;background: #2b3d53;"></span> -->
            <!-- <div class="menuMain">
              <ul class="first-menu-level">
                <li class="menuList menu1">
                  <el-menu-item
                    class="first-menu"
                    index="/center/finance"
                    @click="addTab('finance')"
                    :class="{ isSelectLi: menuIndex === 'finance' }"
                    ><span class="menu-icon finance"></span
                    >财税状况</el-menu-item
                  >
                </li>
              </ul>
            </div> -->
          </el-menu>
        </div>
      </div>
      <div class="content enterprise">
        <!-- <el-tab-pane v-if="tabHome.name !== -1" :label="tabHome.title"> -->
        <!-- </el-tab-pane> -->
        <!-- <div class="scroll__main">

        </div> -->
        <Home :refreshPage="refreshPageFlag" v-if="isHome == true" />
        <el-tabs
          id="tabs"
          class="enterprise__tab"
          style="flex: 1; display: flex; flex-direction: column"
          v-model="editableTabsValue"
          type="border-card"
          closable
          @tab-click="changeTab"
          @tab-remove="removeTab"
          v-if="isHome == false"
        >
          <el-tab-pane
            v-if="tabSubject.name !== -1"
            :label="tabSubject.title"
            :name="tabSubject.name"
            :key="tabSubject.name"
          >
            <subject :refreshPage="refreshPageFlag" />
          </el-tab-pane>
          <el-tab-pane
            v-if="tabEarlyStage.name !== -1"
            :label="tabEarlyStage.title"
            :name="tabEarlyStage.name"
            :key="tabEarlyStage.name"
          >
            <earlyStage :refreshPage="refreshPageFlag" />
          </el-tab-pane>
          <el-tab-pane
            v-if="tabVoucherExport.name !== -1"
            :label="tabVoucherExport.title"
            :name="tabVoucherExport.name"
            :key="tabVoucherExport.name"
          >
            <voucherExport :refreshPage="refreshPageFlag" />
          </el-tab-pane>
          <el-tab-pane
            v-if="tabCustomer.name !== -1"
            :label="tabCustomer.title"
            :name="tabCustomer.name"
            :key="tabCustomer.name"
          >
            <customer :refreshPage="refreshPageFlag" />
          </el-tab-pane>
          <el-tab-pane
            v-if="tabStaff.name !== -1"
            :label="tabStaff.title"
            :name="tabStaff.name"
            :key="tabStaff.name"
          >
            <staff :refreshPage="refreshPageFlag" />
          </el-tab-pane>
          <el-tab-pane
            v-if="tabSupplier.name !== -1"
            :label="tabSupplier.title"
            :name="tabSupplier.name"
            :key="tabSupplier.name"
          >
            <supplier :refreshPage="refreshPageFlag" />
          </el-tab-pane>
          <el-tab-pane
            v-if="tabProject.name !== -1"
            :label="tabProject.title"
            :name="tabProject.name"
            :key="tabProject.name"
          >
            <project :refreshPage="refreshPageFlag" />
          </el-tab-pane>
          <el-tab-pane
            v-if="tabDepartment.name !== -1"
            :label="tabDepartment.title"
            :name="tabDepartment.name"
            :key="tabDepartment.name"
          >
            <department :refreshPage="refreshPageFlag" />
          </el-tab-pane>
          <el-tab-pane
            v-if="tabCommodityType.name !== -1"
            :label="tabCommodityType.title"
            :name="tabCommodityType.name"
            :key="tabCommodityType.name"
          >
            <commodity-type :refreshPage="refreshPageFlag" />
          </el-tab-pane>
          <el-tab-pane
            v-if="tabVoucherInput.name !== -1"
            :label="tabVoucherInput.title"
            :name="tabVoucherInput.name"
            :key="tabVoucherInput.name"
          >
            <voucher-input
              view="input"
              :tableType="'tableInput'"
              :refreshPage="refreshPageFlag"
            />
          </el-tab-pane>
          <!-- <el-tab-pane
            v-if="tabVoucherInputN.name !== -1"
            :label="tabVoucherInputN.title"
            :name="tabVoucherInputN.name"
            :key="tabVoucherInputN.name"
          >
            <voucher-input-n
              view="input"
              :tableType="'tableInput'"
              :refreshPage="refreshPageFlag"
            />
          </el-tab-pane> -->
          <el-tab-pane
            v-if="tabVoucherInfo.name !== -1"
            :label="tabVoucherInfo.title"
            :name="tabVoucherInfo.name"
            :key="tabVoucherInfo.name"
          >
            <voucher-input
              view="detail"
              :tableType="'detail'"
              :refreshPage="refreshPageFlag"
            />
          </el-tab-pane>
          <el-tab-pane
            v-if="tabVoucherQuery.name !== -1"
            :label="tabVoucherQuery.title"
            :name="tabVoucherQuery.name"
            :key="tabVoucherQuery.name"
          >
            <voucher-query :refreshPage="refreshPageFlag" />
          </el-tab-pane>
          <el-tab-pane
            v-if="tabSummaryOfSubjects.name !== -1"
            :label="tabSummaryOfSubjects.title"
            :name="tabSummaryOfSubjects.name"
            :key="tabSummaryOfSubjects.name"
          >
            <summaryOfSubjects :refreshPage="refreshPageFlag" />
          </el-tab-pane>
          <el-tab-pane
            v-if="tabSubjectBalance.name !== -1"
            :label="tabSubjectBalance.title"
            :name="tabSubjectBalance.name"
            :key="tabSubjectBalance.name"
          >
            <subjectBalance :refreshPage="refreshPageFlag" />
          </el-tab-pane>
          <el-tab-pane
            v-if="tabSubjectBalanceQuantity.name !== -1"
            :label="tabSubjectBalanceQuantity.title"
            :name="tabSubjectBalanceQuantity.name"
            :key="tabSubjectBalanceQuantity.name"
          >
            <subject-balance-quantity :refreshPage="refreshPageFlag" />
          </el-tab-pane>
          <el-tab-pane
            v-if="tabSubjectBalanceAuxiliary.name !== -1"
            :label="tabSubjectBalanceAuxiliary.title"
            :name="tabSubjectBalanceAuxiliary.name"
            :key="tabSubjectBalanceAuxiliary.name"
          >
            <subject-balance-auxiliary :refreshPage="refreshPageFlag" />
          </el-tab-pane>
          <el-tab-pane
            v-if="tabLedger.name !== -1"
            :label="tabLedger.title"
            :name="tabLedger.name"
            :key="tabLedger.name"
          >
            <ledger :refreshPage="refreshPageFlag" />
          </el-tab-pane>
          <el-tab-pane
            v-if="tabDetailAccount.name !== -1"
            :label="tabDetailAccount.title"
            :name="tabDetailAccount.name"
            :key="tabDetailAccount.name"
          >
            <detail-account :refreshPage="refreshPageFlag" />
          </el-tab-pane>
          <el-tab-pane
            v-if="tabChronological.name !== -1"
            :label="tabChronological.title"
            :name="tabChronological.name"
            :key="tabChronological.name"
          >
          <chronological :refreshPage="refreshPageFlag" />
          </el-tab-pane>
          <el-tab-pane
            v-if="tabDetailAccountQuantity.name !== -1"
            :label="tabDetailAccountQuantity.title"
            :name="tabDetailAccountQuantity.name"
            :key="tabDetailAccountQuantity.name"
          >
            <detail-account-quantity :refreshPage="refreshPageFlag" />
          </el-tab-pane>
          <el-tab-pane
            v-if="tabDetailAccountAuxiliary.name !== -1"
            :label="tabDetailAccountAuxiliary.title"
            :name="tabDetailAccountAuxiliary.name"
            :key="tabDetailAccountAuxiliary.name"
          >
            <detail-account-auxiliary :refreshPage="refreshPageFlag" />
          </el-tab-pane>
          <el-tab-pane
            v-if="tabProfitForm.name !== -1"
            :label="tabProfitForm.title"
            :name="tabProfitForm.name"
            :key="tabProfitForm.name"
          >
            <profit-form :refreshPage="refreshPageFlag" />
          </el-tab-pane>
          <el-tab-pane
            v-if="tabProfitFormQuarterlyReport.name !== -1"
            :label="tabProfitFormQuarterlyReport.title"
            :name="tabProfitFormQuarterlyReport.name"
            :key="tabProfitFormQuarterlyReport.name"
          >
            <profit-form-quarterly-report :refreshPage="refreshPageFlag" />
          </el-tab-pane>
          <el-tab-pane
            v-if="tabCashFlowForm.name !== -1"
            :label="tabCashFlowForm.title"
            :name="tabCashFlowForm.name"
            :key="tabCashFlowForm.name"
          >
            <cash-flow-form :refreshPage="refreshPageFlag" />
          </el-tab-pane>
          <el-tab-pane
            v-if="tabCashFlowFormQuarterlyReport.name !== -1"
            :label="tabCashFlowFormQuarterlyReport.title"
            :name="tabCashFlowFormQuarterlyReport.name"
            :key="tabCashFlowFormQuarterlyReport.name"
          >
            <cash-flow-form-quarterly-report :refreshPage="refreshPageFlag" />
          </el-tab-pane>
          <el-tab-pane
            v-if="tabBalanceSheet.name !== -1"
            :label="tabBalanceSheet.title"
            :name="tabBalanceSheet.name"
            :key="tabBalanceSheet.name"
          >
            <balance-sheet :refreshPage="refreshPageFlag" />
          </el-tab-pane>

          <el-tab-pane
            v-if="tabExpenseStatistics.name !== -1"
            :label="tabExpenseStatistics.title"
            :name="tabExpenseStatistics.name"
            :key="tabExpenseStatistics.name"
          >
            <ExpenseStatistics :refreshPage="refreshPageFlag" />
          </el-tab-pane>

          <el-tab-pane
            v-if="tabCheckoutTest.name !== -1"
            :label="tabCheckoutTest.title"
            :name="tabCheckoutTest.name"
            :key="tabCheckoutTest.name"
          >
            <checkout-test :refreshPage="refreshPageFlag" />
          </el-tab-pane>
          <!-- addedTax -->
          <el-tab-pane
            v-if="tabPersonalTax.name !== -1"
            :label="tabPersonalTax.title"
            :name="tabPersonalTax.name"
            :key="tabPersonalTax.name"
          >
          <personalTax :refreshPage="refreshPageFlag" />
          </el-tab-pane>
          <el-tab-pane
            v-if="tabAddedTax.name !== -1"
            :label="tabAddedTax.title"
            :name="tabAddedTax.name"
            :key="tabAddedTax.name"
          >
          <addedTax :refreshPage="refreshPageFlag" />
          </el-tab-pane>
          <el-tab-pane
            v-if="tabFixedProperty.name !== -1"
            :label="tabFixedProperty.title"
            :name="tabFixedProperty.name"
            :key="tabFixedProperty.name"
          >
            <fixed-property :refreshPage="refreshPageFlag" />
          </el-tab-pane>
          <el-tab-pane
            v-if="tabAddFixedProperty.name !== -1"
            :label="tabAddFixedProperty.title"
            :name="tabAddFixedProperty.name"
            :key="tabAddFixedProperty.name"
          >
            <add-fixed-property :refreshPage="refreshPageFlag" />
          </el-tab-pane>
          <el-tab-pane
            v-if="tabTypeSet.name !== -1"
            :label="tabTypeSet.title"
            :name="tabTypeSet.name"
            :key="tabTypeSet.name"
          >
            <fixed-type-set :refreshPage="refreshPageFlag" />
          </el-tab-pane>
          <el-tab-pane
            v-if="tabInvisibleTypeSet.name !== -1"
            :label="tabInvisibleTypeSet.title"
            :name="tabInvisibleTypeSet.name"
            :key="tabInvisibleTypeSet.name"
          >
            <invisible-type-set :refreshPage="refreshPageFlag" />
          </el-tab-pane>
          <el-tab-pane
            v-if="tabDetailAmortOfDepre.name !== -1"
            :label="tabDetailAmortOfDepre.title"
            :name="tabDetailAmortOfDepre.name"
            :key="tabDetailAmortOfDepre.name"
          >
            <detail-amort-of-depre :refreshPage="refreshPageFlag" />
          </el-tab-pane>
          <el-tab-pane
            v-if="tabSummaryAmortOfDepre.name !== -1"
            :label="tabSummaryAmortOfDepre.title"
            :name="tabSummaryAmortOfDepre.name"
            :key="tabSummaryAmortOfDepre.name"
          >
            <summary-amort-of-depre :refreshPage="refreshPageFlag" />
          </el-tab-pane>
          <el-tab-pane
            v-if="tabCleanProperty.name !== -1"
            :label="tabCleanProperty.title"
            :name="tabCleanProperty.name"
            :key="tabCleanProperty.name"
          >
            <clean-property :refreshPage="refreshPageFlag" />
          </el-tab-pane>
          <el-tab-pane
            v-if="tabInvisibleProperty.name !== -1"
            :label="tabInvisibleProperty.title"
            :name="tabInvisibleProperty.name"
            :key="tabInvisibleProperty.name"
          >
            <invisible-property :refreshPage="refreshPageFlag" />
          </el-tab-pane>
          <el-tab-pane
            v-if="tabEditFixedProperty.name !== -1"
            :label="tabEditFixedProperty.title"
            :name="tabEditFixedProperty.name"
            :key="tabEditFixedProperty.name"
          >
            <edit-fixed-property :refreshPage="refreshPageFlag" />
          </el-tab-pane>
          <el-tab-pane
            v-if="tabAddInvisibleProperty.name !== -1"
            :label="tabAddInvisibleProperty.title"
            :name="tabAddInvisibleProperty.name"
            :key="tabAddInvisibleProperty.name"
          >
            <add-invisible-property :refreshPage="refreshPageFlag" />
          </el-tab-pane>
          <el-tab-pane
            v-if="tabEditInvisibleProperty.name !== -1"
            :label="tabEditInvisibleProperty.title"
            :name="tabEditInvisibleProperty.name"
            :key="tabEditInvisibleProperty.name"
          >
            <edit-invisible-property :refreshPage="refreshPageFlag" />
          </el-tab-pane>
          <el-tab-pane
            v-if="tabDeferredAssets.name !== -1"
            :label="tabDeferredAssets.title"
            :name="tabDeferredAssets.name"
            :key="tabDeferredAssets.name"
          >
            <deferred-assets :refreshPage="refreshPageFlag" />
          </el-tab-pane>
          <el-tab-pane
            v-if="tabAddDeferredAssets.name !== -1"
            :label="tabAddDeferredAssets.title"
            :name="tabAddDeferredAssets.name"
            :key="tabAddDeferredAssets.name"
          >
            <add-deferred-assets :refreshPage="refreshPageFlag" />
          </el-tab-pane>
          <el-tab-pane
            v-if="tabEditDeferredAssets.name !== -1"
            :label="tabEditDeferredAssets.title"
            :name="tabEditDeferredAssets.name"
            :key="tabEditDeferredAssets.name"
          >
            <edit-deferred-assets :refreshPage="refreshPageFlag" />
          </el-tab-pane>
          <el-tab-pane
            v-if="tabActionLog.name !== -1"
            :label="tabActionLog.title"
            :name="tabActionLog.name"
            :key="tabActionLog.name"
          >
            <action-log :refreshPage="refreshPageFlag" />
          </el-tab-pane>
          <el-tab-pane
            v-if="tabItemType.name !== -1"
            :label="tabItemType.title"
            :name="tabItemType.name"
            :key="tabItemType.name"
          >
            <item-type :refreshPage="refreshPageFlag" />
          </el-tab-pane>
          <el-tab-pane
            v-if="tabIncomeInvoice.name !== -1"
            :label="tabIncomeInvoice.title"
            :name="tabIncomeInvoice.name"
            :key="tabIncomeInvoice.name"
          >
            <income-invoice :refreshPage="refreshPageFlag" />
          </el-tab-pane>
          <el-tab-pane
            v-if="tabOutputInvoice.name !== -1"
            :label="tabOutputInvoice.title"
            :name="tabOutputInvoice.name"
            :key="tabOutputInvoice.name"
          >
            <output-invoice :refreshPage="refreshPageFlag" />
          </el-tab-pane>
          <el-tab-pane
            v-if="tabInvoiceBank.name !== -1"
            :label="tabInvoiceBank.title"
            :name="tabInvoiceBank.name"
            :key="tabInvoiceBank.name"
          >
            <invoice-bank :refreshPage="refreshPageFlag" />
          </el-tab-pane>
          <el-tab-pane
            v-if="tabBankSubjectMatching.name !== -1"
            :label="tabBankSubjectMatching.title"
            :name="tabBankSubjectMatching.name"
            :key="tabBankSubjectMatching.name"
          >
            <bank-subject-matching :refreshPage="refreshPageFlag" />
          </el-tab-pane>
          <el-tab-pane
            v-if="tabCost.name !== -1"
            :label="tabCost.title"
            :name="tabCost.name"
            :key="tabCost.name"
          >
            <cost :refreshPage="refreshPageFlag" />
          </el-tab-pane>
          <el-tab-pane
            v-if="tabCash.name !== -1"
            :label="tabCash.title"
            :name="tabCash.name"
            :key="tabCash.name"
          >
            <cash :refreshPage="refreshPageFlag" />
          </el-tab-pane>
          <el-tab-pane
            v-if="tabOtherFund.name !== -1"
            :label="tabOtherFund.title"
            :name="tabOtherFund.name"
            :key="tabOtherFund.name"
          >
            <other-fund :refreshPage="refreshPageFlag" />
          </el-tab-pane>
          <!-- <el-tab-pane v-if="tabCommodityType.name !== -1" :label="tabCommodityType.title" :name="tabCommodityType.name">
              <commodity-type :refreshPage = "refreshPageFlag"/>
            </el-tab-pane> -->
          <el-tab-pane
            v-if="tabBank.name !== -1"
            :label="tabBank.title"
            :name="tabBank.name"
            :key="tabBank.name"
          >
            <bank :refreshPage="refreshPageFlag" />
          </el-tab-pane>
          <el-tab-pane
            v-if="tabCertification.name !== -1"
            :label="tabCertification.title"
            :name="tabCertification.name"
            :key="tabCertification.name"
          >
            <certification :refreshPage="refreshPageFlag" />
          </el-tab-pane>
          <el-tab-pane
            v-if="tabFinance.name !== -1"
            :label="tabFinance.title"
            :name="tabFinance.name"
            :key="tabFinance.name"
          >
            <finance :refreshPage="refreshPageFlag" />
          </el-tab-pane>
        </el-tabs>
        <vfooter v-if="false" />
      </div>
    </div>
    <customer-info
      ref="customerInfo"
      class="model-box"
      v-if="cusInfoShow"
      :title="title"
      :save="this.save"
      :preview="this.preview"
      :customerStatus="this.customerStatus"
      @close-model="closeModel"
      @submit-customer="submitCustomer"
      :account-selected="this.accountSelectedObj"
    />
  </div>
</template>

<script>
import VoucherInput from "@/views/AccountSet/VoucherInput";
// import VoucherInputN from "@/views/AccountSetN/VoucherInput";
// import VoucherInfo from '@/views/EnterpriseCenter/KeepAccount/VoucherInfo/VoucherInput'
import VoucherQuery from "@/views/EnterpriseCenter/KeepAccount/VoucherQuery/VoucherQuery";
import subject from "@/views/EnterpriseCenter/Subject/Subject";
import earlyStage from "@/views/EnterpriseCenter/EarlyStage/EarlyStage";
import voucherExport from "@/views/EnterpriseCenter/VoucherExport/VoucherExport"; // 凭证导入
import customer from "@/views/EnterpriseCenter/Customer/Customer";
import staff from "@/views/EnterpriseCenter/Staff/Staff";
import project from "@/views/EnterpriseCenter/project/project";
import supplier from "@/views/EnterpriseCenter/Supplier/Supplier";
import department from "@/views/EnterpriseCenter/Department/Department";
// import stock from '@/views/EnterpriseCenter/Stock/Stock'
import Vfooter from "@/components/footer/footer";
import ECHeader from "@/components/header/EnterpriseCenterHeader";
import Home from "@/views/EnterpriseCenter/index/Home";
import Vindex from "@/views/EnterpriseCenter/index/index";
import SummaryOfSubjects from "@/views/EnterpriseCenter/SummaryOfSubjects/SummaryOfSubjects";
import SubjectBalance from "@/views/EnterpriseCenter/SubjectBalance/SubjectBalance";
import SubjectBalanceQuantity from "@/views/EnterpriseCenter/SubjectBalance/SubjectBalanceQuantity";
import SubjectBalanceAuxiliary from "@/views/EnterpriseCenter/SubjectBalance/SubjectBalanceAuxiliary";
import Ledger from "@/views/EnterpriseCenter/AccountBook/Ledger/Ledger";
import DetailAccount from "@/views/EnterpriseCenter/AccountBook/DetailAccount/DetailAccount";
import Chronological from "@/views/EnterpriseCenter/AccountBook/Chronological/Chronological";
// const Chronological = () => import('@/views/EnterpriseCenter/Chronological/Chronological') // 序时账
import DetailAccountQuantity from "@/views/EnterpriseCenter/AccountBook/DetailAccount/DetailAccountQuantity";
import DetailAccountAuxiliary from "@/views/EnterpriseCenter/AccountBook/DetailAccount/DetailAccountAuxiliary";
import ProfitForm from "@/views/EnterpriseCenter/ProfitForm/ProfitForm";
import ProfitFormQuarterlyReport from "@/views/EnterpriseCenter/ProfitForm/ProfitFormQuarterlyReport";
import CashFlowForm from "@/views/EnterpriseCenter/Statement/CashFlowForm/CashFlowForm";
import CashFlowFormQuarterlyReport from "@/views/EnterpriseCenter/Statement/CashFlowForm/CashFlowFormQuarterlyReport";
import BalanceSheet from "@/views/EnterpriseCenter/Statement/BalanceSheet/BalanceSheet";
import ExpenseStatistics from '@/views/EnterpriseCenter/Statement/ExpenseStatistics/ExpenseStatistics';
import checkoutTest from "@/views/EnterpriseCenter/CheckoutTest/checkoutTest";
import personalTax from "@/views/EnterpriseCenter/personalTax/personalTax";
import addedTax from "@/views/EnterpriseCenter/personalTax/addedTax";
import fixedProperty from "@/views/Property/fixedProperty/index"; // 固定资产列表页
import addFixedProperty from "@/views/Property/fixedProperty/add"; // 新增固定资产
import editFixedProperty from "@/views/Property/fixedProperty/edit"; // 编辑固定资产（固定资产卡片）
import fixedTypeSet from "@/views/Property/typeSet/fixedTypeSet";
import invisibleTypeSet from "@/views/Property/typeSet/invisibleTypeSet";
import detailAmortOfDepre from "@/views/Property/detailAmortOfDepre/index";
import summaryAmortOfDepre from "@/views/Property/summaryAmortOfDepre/index";
import cleanProperty from "@/views/Property/cleanProperty/index";
import invisibleProperty from "@/views/Property/invisibleProperty/index"; // 无形资产列表页
import addInvisibleProperty from "@/views/Property/invisibleProperty/add"; // 无形资产新增
import editInvisibleProperty from "@/views/Property/invisibleProperty/edit"; // 无形资产卡片
import deferredAssets from "@/views/Property/deferredAssets"; // 长期待摊费用列表页
import addDeferredAssets from "@/views/Property/deferredAssets/add"; // 长期待摊费用列表页
import editDeferredAssets from "@/views/Property/deferredAssets/edit"; // 长期待摊费用列表页
import actionLog from "@/views/EnterpriseCenter/actionLog/log"; // 操作日志(账套内)
import itemType from "@/views/EnterpriseCenter/ItemType/ItemType"; // 单据类型设置
import incomeInvoice from "@/views/Invoice/IncomeAndOutput/Income/Income"; // 票据--> 进项发票
import outputInvoice from "@/views/Invoice/IncomeAndOutput/Output/Output"; // 票据--> 销项发票
import cost from "@/views/Invoice/Cost/Cost"; // 票据--> 费用
import cash from "@/views/Fund/Cash/cash"; // 资金--> 现金
import otherFund from "@/views/Fund/OtherFund/OtherFund"; // 资金--> 其它货币资金
import commodityType from "@/views/EnterpriseCenter/CommodityType/CommodityType"; // 商品编码设置
import Bank from "@/views/EnterpriseCenter/bank"; // 银行
import Certification from "@/views/Invoice/Certification/Certification"; // 票据--> 费用
import InvoiceBank from "@/views/Invoice/bank/bank"; // 资金银行
import BankSubjectMatching from "@/views/Invoice/bank/bankSubjectMatching"; // 银行科目匹配关系
import Finance from "@/views/EnterpriseCenter/Finance/finance"; // 财税状况
import customerInfo from "@/views/customerManagement/childFile/children/addCustomerModel";

import { mapGetters, mapMutations } from "vuex";
import customerAjax from "@/api/customer/customer";
import SubjectAjax from "@/api/Subject/Subject";
import { message } from "@/components/Message/Message";
import { clickoutside } from "@/tools/tools";

export default {
  data() {
    return {
      title: "",
      cusInfoShow: false,
      keywordVal: {
        keyword: "",
      },
      save: "edit",
      preview: "",
      customerStatus: 1,
      accountSelectedObj: {},
      addStatus: false,
      searchModalShow: false,
      customerList: [],
      timer: null,
      keyword: "",
      total: 0,
      searchPageNo: 1,
      period: "",
      companyName: "",
      editableTabsValue: "",
      editableTabs: [],
      tabIndex: 0,
      tabTitle: "",
      class: "",
      tabFlag: [],
      menuIndex: "",
      refreshPageFlag: "",
      lastActiveMenu: "",
      activeStatus: {
        "1-1": false,
        "1-2": false,
        "1-3": false,
        
        "2-1": false,
        "2-2": false,
        "2-3": false,
        "3-1": false,
      },
      assistShow: true,
      accountShow: true,
      reportFormShow: true,
      editableTabsStorage: [],
      /* ERP客户id */
      erpCustomerId: "",
      isHome: true,
    };
  },
  directives: { clickoutside },
  computed: {
    onRouters() {
      return this.$route.path.replace("/", "");
    },
    ...mapGetters(["getTodoById", "getTodoByIdBox", "getOrder"]),
    tabHome() {
      let tab = this.getTabByContent("index");
      return tab;
    },
    tabSubject() {
      let tab = this.getTabByContent("subject");
      return tab;
    },
    tabEarlyStage() {
      let tab = this.getTabByContent("earlyStage");
      return tab;
    },
    tabVoucherExport() {
      let tab = this.getTabByContent("voucherExport");
      return tab;
    },
    tabCustomer() {
      let tab = this.getTabByContent("customer");
      return tab;
    },
    tabStaff() {
      let tab = this.getTabByContent("staff");
      return tab;
    },
    tabSupplier() {
      let tab = this.getTabByContent("supplier");
      return tab;
    },
    tabProject() {
      let tab = this.getTabByContent("project");
      return tab;
    },
    tabDepartment() {
      let tab = this.getTabByContent("department");
      return tab;
    },
    // tabStock () {
    //   let tab = this.getTabByContent('stock')
    //   return tab
    // },
    tabVoucherInput() {
      let tab = this.getTabByContent("voucherInput");
      return tab;
    },
    // tabVoucherInputN() {
    //   let tab = this.getTabByContent("voucherInputN");
    //   return tab;
    // },
    tabVoucherInfo() {
      let tab = this.getTabByContent("voucherInfo");
      return tab;
    },
    tabVoucherQuery() {
      let tab = this.getTabByContent("voucherQuery");
      return tab;
    },
    tabSummaryOfSubjects() {
      let tab = this.getTabByContent("summaryOfSubjects");
      return tab;
    },
    tabSubjectBalance() {
      let tab = this.getTabByContent("subjectBalance");
      return tab;
    },
    tabSubjectBalanceQuantity() {
      let tab = this.getTabByContent("subjectBalanceQuantity");
      return tab;
    },
    tabSubjectBalanceAuxiliary() {
      let tab = this.getTabByContent("subjectBalanceAuxiliary");
      return tab;
    },
    tabLedger() {
      let tab = this.getTabByContent("ledger");
      return tab;
    },
    tabDetailAccount() {
      let tab = this.getTabByContent("detailAccount");
      return tab;
    },
    tabChronological() {
      let tab = this.getTabByContent("chronological");
      return tab;
    },
    tabDetailAccountQuantity() {
      let tab = this.getTabByContent("detailAccountQuantity");
      return tab;
    },
    tabDetailAccountAuxiliary() {
      let tab = this.getTabByContent("detailAccountAuxiliary");
      return tab;
    },
    tabProfitForm() {
      let tab = this.getTabByContent("profitForm");
      return tab;
    },
    tabProfitFormQuarterlyReport() {
      let tab = this.getTabByContent("profitFormQuarterlyReport");
      return tab;
    },
    tabCashFlowForm() {
      let tab = this.getTabByContent("cashFlowForm");
      return tab;
    },
    tabCashFlowFormQuarterlyReport() {
      let tab = this.getTabByContent("cashFlowFormQuarterlyReport");
      return tab;
    },
    tabBalanceSheet() {
      let tab = this.getTabByContent("balanceSheet");
      return tab;
    },
    tabExpenseStatistics() {
      let tab = this.getTabByContent("expenseStatistics");
      return tab;
    },
    tabCheckoutTest() {
      let tab = this.getTabByContent("checkoutTest");
      return tab;
    },
    // addedTax
    tabPersonalTax() {
      let tab = this.getTabByContent("personalTax");
      return tab;
    },
    tabAddedTax() {
      let tab = this.getTabByContent("addedTax");
      return tab;
    },
    tabFixedProperty() {
      let tab = this.getTabByContent("fixedProperty");
      return tab;
    },
    tabAddFixedProperty() {
      let tab = this.getTabByContent("addFixedProperty");
      return tab;
    },
    tabTypeSet() {
      let tab = this.getTabByContent("fixedTypeSet");
      return tab;
    },
    tabInvisibleTypeSet() {
      let tab = this.getTabByContent("invisibleTypeSet");
      return tab;
    },
    tabDetailAmortOfDepre() {
      let tab = this.getTabByContent("detailAmortOfDepre");
      return tab;
    },
    tabSummaryAmortOfDepre() {
      let tab = this.getTabByContent("summaryAmortOfDepre");
      return tab;
    },
    tabCleanProperty() {
      let tab = this.getTabByContent("cleanProperty");
      return tab;
    },
    tabInvisibleProperty() {
      let tab = this.getTabByContent("invisibleProperty");
      return tab;
    },
    tabEditFixedProperty() {
      let tab = this.getTabByContent("editFixedProperty");
      return tab;
    },
    tabAddInvisibleProperty() {
      let tab = this.getTabByContent("addInvisibleProperty");
      return tab;
    },
    tabEditInvisibleProperty() {
      let tab = this.getTabByContent("editInvisibleProperty");
      return tab;
    },
    tabDeferredAssets() {
      let tab = this.getTabByContent("deferredAssets");
      return tab;
    },
    tabAddDeferredAssets() {
      let tab = this.getTabByContent("addDeferredAssets");
      return tab;
    },
    tabEditDeferredAssets() {
      let tab = this.getTabByContent("editDeferredAssets");
      return tab;
    },
    tabActionLog() {
      let tab = this.getTabByContent("actionLog");
      return tab;
    },
    tabItemType() {
      let tab = this.getTabByContent("itemType");
      return tab;
    },
    tabIncomeInvoice() {
      let tab = this.getTabByContent("incomeInvoice");
      return tab;
    },
    tabOutputInvoice() {
      let tab = this.getTabByContent("outputInvoice");
      return tab;
    },
    tabCost() {
      let tab = this.getTabByContent("cost");
      return tab;
    },
    tabCash() {
      let tab = this.getTabByContent("cash");
      return tab;
    },
    tabOtherFund() {
      let tab = this.getTabByContent("otherFund");
      return tab;
    },
    tabCommodityType() {
      let tab = this.getTabByContent("commodityType");
      return tab;
    },
    tabBank() {
      let tab = this.getTabByContent("bank");
      return tab;
    },
    tabCertification() {
      let tab = this.getTabByContent("certification");
      return tab;
    },
    tabInvoiceBank() {
      let tab = this.getTabByContent("invoiceBank");
      return tab;
    },
    tabBankSubjectMatching() {
      let tab = this.getTabByContent("bankSubjectMatching");
      return tab;
    },
    tabFinance() {
      let tab = this.getTabByContent("finance");
      return tab;
    },
  },
  components: {
    VoucherInput,
    // VoucherInputN,
    // VoucherInfo,
    VoucherQuery,
    subject,
    earlyStage,
    voucherExport, // 凭证导入
    customer,
    supplier,
    department,
    // stock,
    Vfooter,
    ECHeader,
    staff,
    SummaryOfSubjects,
    SubjectBalance,
    SubjectBalanceQuantity,
    SubjectBalanceAuxiliary,
    Ledger,
    DetailAccount,
    Chronological,
    DetailAccountQuantity,
    DetailAccountAuxiliary,
    project,
    Vindex,
    ProfitForm,
    ProfitFormQuarterlyReport,
    CashFlowForm,
    CashFlowFormQuarterlyReport,
    BalanceSheet,
    ExpenseStatistics,
    checkoutTest,
    personalTax,
    addedTax,
    fixedProperty,
    addFixedProperty,
    fixedTypeSet,
    invisibleTypeSet,
    detailAmortOfDepre,
    summaryAmortOfDepre,
    cleanProperty,
    invisibleProperty,
    editFixedProperty,
    addInvisibleProperty,
    editInvisibleProperty,
    deferredAssets,
    addDeferredAssets,
    editDeferredAssets,
    actionLog,
    itemType,
    incomeInvoice, // 进项发票
    outputInvoice, // 销项发票
    cost,
    cash, // 资金--> 现金
    otherFund,
    commodityType, // 商品编码
    Bank,
    Certification,
    InvoiceBank,
    BankSubjectMatching,
    Finance,
    customerInfo,
    Home,
  },
  watch: {
    onRouters: {
      handler: function (newValue, oldValue) {
        this.menuIndex = newValue.split("/")[1];
        let routeFlag = newValue.split("/")[1];
        if (routeFlag !== undefined && routeFlag !== "index" && routeFlag) {
          this.addTab(routeFlag);
        }
        let storage = sessionStorage.getItem("tabsArr");
        if (storage) {
          this.editableTabs = JSON.parse(storage);
          this.editableTabs.forEach((item) => {
            if (routeFlag && routeFlag === item.content) {
              this.editableTabsValue = item.name;
            }
          });
        }
      },
    },
    getOrder: function (val) {
      if (val.isEnd === "success") {
        this.querySubjectLevel();
      }
    },
  },
  created() {
    this.isHome = true;

    let ifReset = localStorage.getItem("ifNull");
    let storage = sessionStorage.getItem("tabsArr");
    if (ifReset === "success") {
      this.editableTabs = JSON.parse(storage).splice(0, 1);
      // this.tabFlag = ["index"];
      sessionStorage.setItem("tabsArr", JSON.stringify(this.editableTabs));
      sessionStorage.setItem("tabsFlagArr", JSON.stringify(this.tabFlag));
      localStorage.removeItem("ifNull");
    }
    let currentRoute = this.$router.history.current.fullPath;
    if (
      currentRoute === "/center" ||
      currentRoute === "/center/" ||
      currentRoute === "/center/index"
    ) {
      this.$router.push({ path: "/center/index" });
      // this.addTab("index");
    } else {
      this.$router.push({ path: currentRoute });
      this.isHome = false;
    }
    if (
      this.onRouters === "center/" ||
      this.onRouters === "center"
    ) {
      // 一级路由 刷新清空页签数组/页签标识数组 并存入localStorage
      this.editableTabs = [];
      this.tabFlag = [];
      sessionStorage.setItem("tabsArr", JSON.stringify(this.editableTabs));
      sessionStorage.setItem("tabsFlagArr", JSON.stringify(this.tabFlag));
    }
    let routeFlag = this.onRouters.split("/")[1];
    this.menuIndex = routeFlag;
    let storage2 = sessionStorage.getItem("tabsArr");
    if (storage2) {
      this.editableTabs = JSON.parse(storage2);
      this.editableTabs.forEach((item) => {
        if (routeFlag && routeFlag === item.content) {
          this.editableTabsValue = item.name;
          this.refreshPageFlag = item.content;
        }
      });
    }
    this.querySubjectLevel();
    this.queryCustomerList();
  },
  mounted() {
    // if (this.$refs.reportFormBox.children.length === 0) {
    //   this.reportFormShow = false
    // }
    // if (this.$refs.accountBox.children.length === 0) {
    //   this.accountShow = false
    // }
    // if (this.$refs.assistBox.children.length === 0) {
    //   this.assistShow = false
    // }
  },
  methods: {
    ...mapMutations(["updateTaxNatures"]),
    toHome() {
      this.$router.push({ path: "/center/index" });
      this.isHome = true;
    },
    closeSearchModal(env) {
      if (env.target.id === "") {
        this.searchModalShow = false;
      }
    },
    toggleSearchModal() {
      this.searchModalShow = !this.searchModalShow;
      if (this.searchModalShow) {
        this.$nextTick(() => {
          const searchDiv = document.querySelector(".search-panel");
          searchDiv.addEventListener("scroll", () => {
            if (!this.addStatus) {
              let clientH = searchDiv.clientHeight;
              let scrollT = searchDiv.scrollTop;
              let scrollH = searchDiv.scrollHeight;
              if (clientH + scrollT >= scrollH) {
                this.addStatus = true;
                this.searchPageNo++;
                const data = {
                  keyword: this.keyword,
                  pageNum: this.searchPageNo,
                };
                if (
                  this.total !== 0 &&
                  this.searchPageNo <= Math.ceil(this.total / 10)
                ) {
                  this.getCustomerList(data);
                }
              }
            }
          });
        });
      }
    },
    queryCustomerList() {
      this.customerList = [];
      this.searchPageNo = 1;
      this.timer = setTimeout(() => {
        this.getCustomerList(
          { keyword: this.keyword, pageNum: this.searchPageNo },
          true
        );
      }, 300);
    },
    getCustomerList(data, flag = false) {
      let $this = this;
      SubjectAjax.listSimple(data).then((res) => {
        if (res.code === 200) {
          if (flag) {
            this.customerList = [];
          }
          this.total = res.data.total;
          if (res.data.length < 10 || data.pageNum * 10 === this.total) {
            $this.addStatus = true;
          } else {
            $this.addStatus = false;
          }
          res.data.list.forEach((element, index) => {
            this.customerList.push(element);
          });
        } else if (res.code === 400) {
          message({
            message: res.message,
            type: "error",
          });
        }
      });
    },
    selectCustomer(customer) {
      customerAjax
        .enterOfBooks(customer.id, customer.accountSetId)
        .then((res) => {
          if (res.code === 200) {
            localStorage.setItem("ifNull", "success");
            sessionStorage.setItem("taxNature", customer.taxNature);
            this.$router.push({ path: "/center/index" });
            location.reload();
          }
        });
    },
    querySubjectLevel() {
      // 查询列表
      SubjectAjax.SubjectLevel().then((res) => {
        if (res.code === 200) {
          let newVal = res.data.currentPeriod.toString();
          this.period = `${newVal.substring(0, 4)}${newVal
            .split(newVal.substring(0, 4))
            .join("-")}期`;
          this.companyName = res.data.name;
          this.updateTaxNatures(res.data.taxNature);
          sessionStorage.setItem("customerId", res.data.customerId);
          sessionStorage.setItem("currentPeriod", res.data.currentPeriod);
          this.companyId = res.data.customerId;
          this.erpCustomerId = res.data.erpCustomerId;
        }
      });
    },
    accountDetail() {
      this.keywordVal.keyword = "";
      let isLogin = sessionStorage.getItem("isLogin");
      if (isLogin) {
        this.cusInfoShow = true;
      } else {
        top.postMessage({ customer: { id: this.erpCustomerId } }, "*");
        return false;
      }

      this.preview = "edit";
      this.title = "预览客户信息-" + this.companyName;
      // 客户详情接口
      customerAjax.customerInfo(this.companyId).then((res) => {
        if (res.code === 200) {
          this.customerStatus = res.data.status;
          this.accountSelectedObj = res.data;
        }
      });
    },
    customerInfoFn(data) {
      // 客户详情接口
      customerAjax.customerInfo(data).then((res) => {
        if (res.code === 200) {
          this.accountSelectedObj = res.data;
        }
      });
    },
    closeModel() {
      let obj = {};
      obj.isEnd = "success";
      this.$store.commit("SET_ORDER", obj);
      this.cusInfoShow = false;
    },
    submitCustomer(data, preview, type, saveType) {
      if (saveType === "basic") {
        if (preview === "create") {
          customerAjax.addCustomer(data, "add").then((res) => {
            this.$refs.customerInfo.closeLoadding();
            if (res.code === 200) {
              this.$message({
                message: "创建成功！",
                type: "success",
              });
              this.preview = "edit";
              this.customerInfoFn(res.data.id);
              if (type === "save") {
                this.save = "save";
                this.cusInfoShow = true;
              } else {
                let obj = {};
                obj.isEnd = "success";
                this.$store.commit("SET_ORDER", obj);
                this.cusInfoShow = false;
              }
              this.getCustomerList(
                { keyword: this.keyword, pageNum: this.searchPageNo },
                true
              );
            } else if (res.code === 400) {
              this.$message({
                message: `${res.message}`,
                type: "error",
              });
            }
          });
        } else if (preview === "edit") {
          customerAjax.addCustomer(data, "update").then((res) => {
            this.$refs.customerInfo.closeLoadding();
            if (res.code === 200) {
              this.$message({
                message: "保存成功！",
                type: "success",
              });
              if (type === "save") {
                this.cusInfoShow = true;
              } else {
                let obj = {};
                obj.isEnd = "success";
                this.$store.commit("SET_ORDER", obj);
                this.cusInfoShow = false;
              }
              this.customerInfoFn(data.id);
              this.getCustomerList(
                { keyword: this.keyword, pageNum: this.searchPageNo },
                true
              );
            } else if (res.code === 400) {
              this.$message({
                message: `${res.message}`,
                type: "error",
              });
            }
          });
        }
      } else if (saveType === "taxInformation" || saveType === "taxCategory") {
        customerAjax.taxInformationUpdate(data).then((res) => {
          this.$refs.customerInfo.closeLoadding();
          if (res.code === 200) {
            this.$message({
              message: "保存成功！",
              type: "success",
            });
            this.customerInfoFn(data.customerId);
            if (type === "save") {
              this.save = "save";
              this.cusInfoShow = true;
            } else {
              let obj = {};
              obj.isEnd = "success";
              this.$store.commit("SET_ORDER", obj);
              this.cusInfoShow = false;
            }
          } else if (res.code === 400) {
            this.$message({
              message: `${res.message}`,
              type: "error",
            });
          }
        });
      } else if (saveType === "accountInformation") {
        customerAjax.accountSetSettingUpdate(data).then((res) => {
          this.$refs.customerInfo.closeLoadding();
          if (res.code === 200) {
            this.$store.dispatch("common/getAccountSet");
            this.$message({
              message: "保存成功！",
              type: "success",
            });
            this.customerInfoFn(data.customerId);
            if (type === "save") {
              this.save = "save";
              this.cusInfoShow = true;
            } else {
              let obj = {};
              obj.isEnd = "success";
              this.$store.commit("SET_ORDER", obj);
              this.cusInfoShow = false;
            }
          } else if (res.code === 400) {
            this.$message({
              message: `${res.message}`,
              type: "error",
            });
          }
        });
      } else if (
        saveType === "contactsPeople" ||
        saveType === "serviceDynamics"
      ) {
        this.loading = false;
        if (type === "save") {
          this.save = "save";
          this.cusInfoShow = true;
        } else {
          this.cusInfoShow = false;
        }
      }
    },
    openSubmenu(index, indexPath) {
      if (
        index === "1-1" ||
        index === "1-2" ||
        index === "1-3" ||
        index === "2-1" ||
        index === "2-2" ||
        index === "2-3" ||
        index === "3-1"
      ) {
        this.activeStatus[this.lastActiveMenu] = false;
        this.activeStatus[index] = true;
        this.lastActiveMenu = index;
      }
    },
    closeSubmenu(index, indexPath) {
      this.activeStatus[index] = false;
    },
    getTabByContent(content) {
      console.log(111);
      let tab = this.editableTabs.find((item) => {
        return item.content === content;
      });
      return tab === undefined ? { name: -1 } : tab;
    },
    addTab(targetName) {
      this.isHome ? (this.isHome = false) : (this.isHome = this.isHome);
      this.refreshPageFlag = targetName;
      switch (targetName) {
        case "subject":
          this.tabTitle = "科目";
          break;
        case "earlyStage":
          this.tabTitle = "期初";
          break;
        case "voucherExport":
          this.tabTitle = "凭证导入";
          break;
        case "index":
          this.tabTitle = "首页";
          break;
        case "customer":
          this.tabTitle = "客户";
          break;
        case "staff":
          this.tabTitle = "员工";
          break;
        case "supplier":
          this.tabTitle = "供应商";
          break;
        case "project":
          this.tabTitle = "项目";
          break;
        case "department":
          this.tabTitle = "部门";
          break;
        case "commodityType":
          this.tabTitle = "存货及服务";
          break;
        case "voucherInput":
          this.tabTitle = "凭证录入";
          break;
        // case "voucherInputN":
        //   this.tabTitle = "凭证录入N";
        //   break;
        case "voucherInfo":
          this.tabTitle = "凭证信息";
          break;
        case "voucherQuery":
          this.tabTitle = "凭证查询";
          break;
        case "ledger":
          this.tabTitle = "总账";
          break;
        case "detailAccount":
          this.tabTitle = "明细账";
          break;
        case "chronological":
          this.tabTitle = "序时账";
          break;
        case "detailAccountQuantity":
          this.tabTitle = "数量核算明细账";
          break;
        case "detailAccountAuxiliary":
          this.tabTitle = "辅助核算明细账";
          break;
        case "summaryOfSubjects":
          this.tabTitle = "科目汇总表";
          break;
        case "subjectBalance":
          this.tabTitle = "科目余额表";
          break;
        case "subjectBalanceQuantity":
          this.tabTitle = "数量核算余额表";
          break;
        case "subjectBalanceAuxiliary":
          this.tabTitle = "辅助核算余额表";
          break;
        case "profitForm":
          this.tabTitle = "利润表";
          break;
        case "profitFormQuarterlyReport":
          this.tabTitle = "利润表季报";
          break;
        case "cashFlowForm":
          this.tabTitle = "现金流量表";
          break;
        case "cashFlowFormQuarterlyReport":
          this.tabTitle = "现金流量表季报";
          break;
        case "balanceSheet":
          this.tabTitle = "资产负债表";
          break;
        case "expenseStatistics":
          this.tabTitle = "费用统计表";
          break;
        case "checkoutTest":
          this.tabTitle = "结账";
          break;
          // addedTax
        case "personalTax":
          this.tabTitle = "企业所得税";
          break;
        case "addedTax":
          this.tabTitle = "增值税及附加税";
          break;
        case "fixedProperty":
          this.tabTitle = "固定资产";
          break;
        case "addFixedProperty":
          this.tabTitle = "新增固定资产";
          break;
        case "fixedTypeSet":
          this.tabTitle = "固定资产类别设置";
          break;
        case "invisibleTypeSet":
          this.tabTitle = "无形资产类别设置";
          break;
        case "detailAmortOfDepre":
          this.tabTitle = "折旧摊销明细";
          break;
        case "summaryAmortOfDepre":
          this.tabTitle = "折旧摊销汇总";
          break;
        case "cleanProperty":
          this.tabTitle = "资产清理处置";
          break;
        case "invisibleProperty":
          this.tabTitle = "无形资产";
          break;
        case "editFixedProperty":
          this.tabTitle = "固定资产卡片";
          break;
        case "addInvisibleProperty":
          this.tabTitle = "新增无形资产";
          break;
        case "editInvisibleProperty":
          this.tabTitle = "无形资产卡片";
          break;
        case "deferredAssets":
          this.tabTitle = "长期待摊费用";
          break;
        case "addDeferredAssets":
          this.tabTitle = "新增长期待摊费用";
          break;
        case "editDeferredAssets":
          this.tabTitle = "长期待摊费用卡片";
          break;
        case "actionLog":
          this.tabTitle = "操作日志";
          sessionStorage.setItem("logType", "1");
          break;
        case "itemType":
          this.tabTitle = "单据类型设置";
          break;
        case "incomeInvoice":
          this.tabTitle = "进项发票";
          break;
        case "outputInvoice":
          this.tabTitle = "销项发票";
          break;
        case "cost":
          this.tabTitle = "费用";
          break;
        case "cash":
          this.tabTitle = "现金";
          break;
        case "otherFund":
          this.tabTitle = "其他货币资金";
          break;
        // case 'commodityType':
        //   this.tabTitle = '商品编码设置'
        //   break
        case "bank":
          this.tabTitle = "银行";
          break;
        case "certification":
          this.tabTitle = "认证抵扣";
          break;
        case "invoiceBank":
          this.tabTitle = "银行 ";
          break;
        case "bankSubjectMatching":
          this.tabTitle = "银行科目匹配关系 ";
          break;
        case "finance":
          this.tabTitle = "财税状况";
          break;
        default:
          break;
      }
      let tabstorage = sessionStorage.getItem("tabsFlagArr");
      if (tabstorage) {
        this.tabFlag = JSON.parse(tabstorage);
      }
      if (this.tabFlag.includes(targetName)) {
        return false;
      }
      this.tabFlag.push(targetName);
      sessionStorage.setItem("tabsFlagArr", JSON.stringify(this.tabFlag));

      let newTabName = ++this.tabIndex + "";
      this.editableTabs.push({
        title: this.tabTitle,
        name: newTabName,
        content: targetName,
      });
      this.editableTabs = this.editableTabs.map((item, index) => {
        item.name = ++index + "";
        return item;
      });
      sessionStorage.setItem("tabsArr", JSON.stringify(this.editableTabs));
      this.editableTabsValue = newTabName;
    },
    removeTab(targetName) {
      // if (targetName === "1") {
      //   return;
      // }
      let tabs = this.editableTabs;
      let activeName = this.editableTabsValue;
      let activeKey = null;
      if(tabs.length<=1){
        this.$router.push({ path: "/center/index" });
        location.reload();
      }
      for (let index in tabs) {
        let tab = tabs[index];
        if (activeName === targetName) {
          if (tab.name === targetName) {
            let nextTab = tabs[parseInt(index) + 1] || tabs[index - 1];
            if (nextTab) {
              activeKey = nextTab.content;
              break;
            }
          }
        } else if (tab.name === activeName) {
          activeKey = tab.content;
          break;
        }
      }
      this.editableTabs = tabs.filter((tab) => tab.name !== targetName);
      let tabstorage = sessionStorage.getItem("tabsFlagArr"); // 刷新页面 取页签标识 删除localStorage中标识 并存最新localStorage
      if (tabstorage) {
        this.tabFlag = JSON.parse(tabstorage);
        this.tabFlag = this.tabFlag.filter((item, index) => {
          return index + 1 !== parseInt(targetName);
        });
        this.editableTabs = this.editableTabs.map((item, index) => {
          item.name = ++index + "";
          return item;
        });
        sessionStorage.setItem("tabsFlagArr", JSON.stringify(this.tabFlag));
      }

      sessionStorage.setItem("tabsArr", JSON.stringify(this.editableTabs));

      if (activeKey != null) {
        for (let index in this.editableTabs) {
          let tab = this.editableTabs[index];
          if (activeKey === tab.content) {
            this.editableTabsValue = tab.name;
            break;
          }
        }
        this.$router.push({ path: `/center/${activeKey}` });
      } else {
        this.$router.push({ path: "/center" });
      }
    },
    changeTab(val) {
      switch (val.label) {
        case "科目":
          this.$router.push({ path: "/center/subject" });
          this.refreshPageFlag = "subject";
          break;
        case "期初":
          this.$router.push({ path: "/center/earlyStage" });
          this.refreshPageFlag = "earlyStage";
          break;
        case "凭证导入":
          this.$router.push({ path: "/center/voucherExport" });
          this.refreshPageFlag = "voucherExport";
          break;
        // case "首页":
        //   this.$router.push({ path: "/center/index" });
        //   this.refreshPageFlag = "index";
        //   break;
        case "客户":
          this.$router.push({ path: "/center/customer" });
          this.refreshPageFlag = "customer";
          break;
        case "员工":
          this.$router.push({ path: "/center/staff" });
          this.refreshPageFlag = "staff";
          break;
        case "供应商":
          this.$router.push({ path: "/center/supplier" });
          this.refreshPageFlag = "supplier";
          break;
        case "项目":
          this.$router.push({ path: "/center/project" });
          this.refreshPageFlag = "project";
          break;
        case "部门":
          this.$router.push({ path: "/center/department" });
          this.refreshPageFlag = "department";
          break;
        case "存货及服务":
          this.$router.push({ path: "/center/commodityType" });
          this.refreshPageFlag = "commodityType";
          break;
        case "凭证录入":
          this.$router.push({ path: "/center/voucherInput" });
          this.refreshPageFlag = "voucherInput";
          break;
        // case "凭证录入N":
        //   this.$router.push({ path: "/center/voucherInputN" });
        //   this.refreshPageFlag = "voucherInputN";
        //   break;
        case "凭证信息":
          this.$router.push({ path: "/center/voucherInfo" });
          this.refreshPageFlag = "voucherInfo";
          break;
        case "凭证查询":
          this.$router.push({ path: "/center/voucherQuery" });
          this.refreshPageFlag = "voucherQuery";
          break;
        case "总账":
          this.$router.push({ path: "/center/ledger" });
          this.refreshPageFlag = "ledger";
          break;
        case "明细账":
          this.$router.push({ path: "/center/detailAccount" });
          this.refreshPageFlag = "detailAccount";
          break;
        case "序时账":
          this.$router.push({ path: "/center/chronological" });
          this.refreshPageFlag = "chronological";
          break;
        case "数量核算明细账":
          this.$router.push({
            path: "/center/detailAccountQuantity",
          });
          this.refreshPageFlag = "detailAccountQuantity";
          break;
        case "辅助核算明细账":
          this.$router.push({
            path: "/center/detailAccountAuxiliary",
          });
          this.refreshPageFlag = "detailAccountAuxiliary";
          break;
        case "科目汇总表":
          this.$router.push({ path: "/center/summaryOfSubjects" });
          this.refreshPageFlag = "summaryOfSubjects";
          break;
        case "科目余额表":
          this.$router.push({ path: "/center/subjectBalance" });
          this.refreshPageFlag = "subjectBalance";
          break;
        case "数量核算余额表":
          this.$router.push({
            path: "/center/subjectBalanceQuantity",
          });
          this.refreshPageFlag = "subjectBalanceQuantity";
          break;
        case "辅助核算余额表":
          this.$router.push({
            path: "/center/subjectBalanceAuxiliary",
          });
          this.refreshPageFlag = "subjectBalanceAuxiliary";
          break;
        case "利润表":
          this.$router.push({ path: "/center/profitForm" });
          this.refreshPageFlag = "profitForm";
          break;
        case "利润表季报":
          this.$router.push({ path: "/center/profitFormQuarterlyReport" });
          this.refreshPageFlag = "profitFormQuarterlyReport";
          break;
        case "现金流量表":
          this.$router.push({ path: "/center/cashFlowForm" });
          this.refreshPageFlag = "cashFlowForm";
          break;
        case "现金流量表季报":
          this.$router.push({ path: "/center/cashFlowFormQuarterlyReport" });
          this.refreshPageFlag = "cashFlowFormQuarterlyReport";
          break;
        case "资产负债表":
          this.$router.push({ path: "/center/balanceSheet" });
          this.refreshPageFlag = "balanceSheet";
          break;

        case "费用统计表":
          this.$router.push({ path: "/center/expenseStatistics" });
          this.refreshPageFlag = "expenseStatistics";
          break;

        case "结账":
          this.$router.push({ path: "/center/checkoutTest" });
          this.refreshPageFlag = "checkoutTest";
          break;
          // addedTax
        case "企业所得税":
          this.$router.push({ path: "/center/personalTax/personalTax" });
          this.refreshPageFlag = "personalTax";
          break;
        case "增值税及附加税":
          this.$router.push({ path: "/center/personalTax/addedTax" });
          this.refreshPageFlag = "addedTax";
          break;
        case "固定资产":
          this.$router.push({ path: "/center/fixedProperty" });
          this.refreshPageFlag = "fixedProperty";
          break;
        case "新增固定资产":
          this.$router.push({ path: "/center/addFixedProperty" });
          this.refreshPageFlag = "addFixedProperty";
          break;
        case "固定资产类别设置":
          this.$router.push({ path: "/center/fixedTypeSet" });
          this.refreshPageFlag = "fixedTypeSet";
          break;
        case "无形资产类别设置":
          this.$router.push({ path: "/center/invisibleTypeSet" });
          this.refreshPageFlag = "invisibleTypeSet";
          break;
        case "折旧摊销明细":
          this.$router.push({ path: "/center/detailAmortOfDepre" });
          this.refreshPageFlag = "detailAmortOfDepre";
          break;
        case "折旧摊销汇总":
          this.$router.push({ path: "/center/summaryAmortOfDepre" });
          this.refreshPageFlag = "summaryAmortOfDepre";
          break;
        case "资产清理处置":
          this.$router.push({ path: "/center/cleanProperty" });
          this.refreshPageFlag = "cleanProperty";
          break;
        case "无形资产":
          this.$router.push({ path: "/center/invisibleProperty" });
          this.refreshPageFlag = "invisibleProperty";
          break;
        case "固定资产卡片":
          this.$router.push({ path: "/center/editFixedProperty" });
          this.refreshPageFlag = "editFixedProperty";
          break;
        case "新增无形资产":
          this.$router.push({ path: "/center/addInvisibleProperty" });
          this.refreshPageFlag = "addInvisibleProperty";
          break;
        case "无形资产卡片":
          this.$router.push({
            path: "/center/editInvisibleProperty",
          });
          this.refreshPageFlag = "editInvisibleProperty";
          break;
        case "长期待摊费用":
          this.$router.push({ path: "/center/deferredAssets" });
          this.refreshPageFlag = "deferredAssets";
          break;
        case "新增长期待摊费用":
          this.$router.push({ path: "/center/addDeferredAssets" });
          this.refreshPageFlag = "addDeferredAssets";
          break;
        case "长期待摊费用卡片":
          this.$router.push({ path: "/center/editDeferredAssets" });
          this.refreshPageFlag = "editDeferredAssets";
          break;
        case "操作日志":
          this.$router.push({ path: "/center/actionLog" });
          this.refreshPageFlag = "actionLog";
          break;
        case "单据类型设置":
          this.$router.push({ path: "/center/itemType" });
          this.refreshPageFlag = "itemType";
          break;
        case "进项发票":
          this.$router.push({ path: "/center/incomeInvoice" });
          this.refreshPageFlag = "incomeInvoice";
          break;
        case "销项发票":
          this.$router.push({ path: "/center/outputInvoice" });
          this.refreshPageFlag = "outputInvoice";
          break;
        case "费用":
          this.$router.push({ path: "/center/cost" });
          this.refreshPageFlag = "cost";
          break;
        case "现金":
          this.$router.push({ path: "/center/cash" });
          this.refreshPageFlag = "cash";
          break;
        case "其它他币资金":
          this.$router.push({ path: "/center/otherFund" });
          this.refreshPageFlag = "otherFund";
          break;
        // case '商品编码设置':
        //   this.$router.push({path: '/center/commodityType'})
        //   this.refreshPageFlag = 'commodityType'
        //   break
        case "银行":
          this.$router.push({ path: "/center/bank" });
          this.refreshPageFlag = "bank";
          break;
        case "认证抵扣":
          this.$router.push({ path: "/center/certification" });
          this.refreshPageFlag = "certification";
          break;
        case "银行 ":
          this.$router.push({ path: "/center/invoiceBank" });
          this.refreshPageFlag = "invoiceBank";
          break;
        case "银行科目匹配关系 ":
          this.$router.push({ path: "/center/bankSubjectMatching" });
          this.refreshPageFlag = "bankSubjectMatching";
          break;
        case "财税状况":
          this.$router.push({ path: "/center/finance" });
          this.refreshPageFlag = "finance";
          break;
        default:
          break;
      }
    },
  },
};
</script>

<style lang="less">
.enterprise {
  background: #f5f9fc;
  // overflow: scroll !important;

  .enterprise__tab {
    margin: 20px;
  }
  .el-tabs__header {
    // height: 44px;
    .el-tabs__nav-wrap {
      // height: 44px;
      padding: 0;
      margin: 0;
      .el-tabs__nav-scroll {
        // height: 34px;
        .el-tabs__nav {
          display: flex;
          flex-direction: row;
          .el-tabs__item {
            background-color: #ffffff;
            border: 1px solid #f0f0f0;
            color: #333;
            min-width: 112px;
            font-size: 12px;
            display: flex;
            align-items: center;
            padding: 12px 16px;
            justify-content: space-between;
            &.is-active {
              color: #7693fa;
            }
            &:hover {
              color: #7693fa;
            }
          }
        }
      }
    }
  }

  .el-tabs__nav-prev {
    left: 0;
    top: 10px;
    display: block;
    height: 34px;
    line-height: 34px;
    z-index: 3;
    &:hover {
      color: #5478fb;
      border-radius: 4px 4px 0px 0px;
    }
  }
  .el-tabs__nav-next {
    right: 10px;
    display: block;
    height: 34px;
    line-height: 34px;
    top: 10px;
    z-index: 3;
    &:hover {
      color: #5478fb;
      border-radius: 4px 4px 0px 0px;
    }
  }
  .el-tabs--border-card {
    height: 100%;
    background: #f5f9fc;
  }
  .el-tabs--border-card > .el-tabs__header {
    background: #f3f3f4;
    border: none;
  }
  .el-tabs--border-card > .el-tabs__content {
    padding: 0;
  }
  .el-tabs--top .el-tabs__item:hover {
    border-left: none !important;
  }
  .el-tabs__item .el-icon-close {
    transition: all 10000000s cubic-bezier(0.645, 0.045, 0.355, 1);
  }
  .el-tabs--card > .el-tabs__header .el-tabs__item.is-active.is-closable {
    background: #fff !important;
  }
  .el-tabs--card > .el-tabs__header .el-tabs__item .el-icon-close {
    overflow: inherit !important;
    right: 0;
  }
  // .el-tabs--card > .el-tabs__header .el-tabs__item:first-child .el-icon-close {
  //   display: none;
  // }
  // .el-tabs__item:first-child .el-icon-close:before {
  //   content: "";
  // }
  // .el-tabs__item:first-child .el-icon-close:hover {
  //   background: #fff;
  // }
  //
  .export-print {
    top: 160px;
    right: 20px;
  }
  // 资产 tab套tab
  .amort-depre-box {
    padding: 30px 20px 0 20px;
    .fixed-panel {
      background: #ffffff;
    }
    .invisible-panel {
      background: #ffffff;
    }
    .long-term-panel {
      background: #ffffff;
    }
    .el-tabs {
      height: 100%;
      .el-tabs__content {
        height: calc(100% - 104px);
        .custom-property-search {
          margin-bottom: 92px;
          height: 32px;

          .el-form {
            height: 32px;
          }
        }
        button {
          height: 32px;
          line-height: 32px;
        }
      }
      .el-tab-pane {
        height: 100%;
      }
    }
    .el-form {
      display: flex;
      align-items: center;

      .el-form-item {
        display: flex;
        align-items: center;
        .el-form-item__label {
          flex: none;
        }
        .el-input__prefix {
          display: none;
        }
      }
    }
    .el-tabs__header {
      display: flex;
      justify-content: center;
      margin-bottom: 30px;
      .el-tabs__nav-wrap {
        padding: 0;
        margin: 0;
        &:after {
          content: none;
        }
        .el-tabs__nav-scroll {
          .el-tabs__nav {
            background: #f5f5f5;
            border-radius: 25px;
            color: #6c6c6c;
            font-size: 14px;
            margin: 0 auto;
            .el-tabs__active-bar {
              width: 120px !important;
              margin-left: -16px;
              height: 38px;
              border-radius: 25px;
              background: #5478fb;
            }
            .el-tabs__item {
              z-index: 2;
              margin: 0;
              width: 120px;
              height: 38px;
              line-height: 38px;
              background: transparent;
              border: none;
              &.is-active {
                color: #ffffff;
              }
              span {
                margin: 0 auto;
              }
            }
          }
        }
      }
    }
  }
  .summary-property-box {
    padding: 30px 20px 0 20px;
    .summary-fixed-panel {
      background: #ffffff;
    }
    .summary-invisible-panel {
      background: #ffffff;
    }
    .el-tabs {
      height: 100%;
      .el-tabs__content {
        height: calc(100% - 104px);
        .custom-property-search {
          margin-bottom: 92px;
          height: 32px;
          .el-form {
            height: 32px;
          }
        }
        button {
          height: 32px;
          line-height: 32px;
        }
      }
      .el-tab-pane {
        height: 100%;
      }
    }
    .el-form {
      display: flex;
      align-items: center;

      .el-form-item {
        display: flex;
        align-items: center;
        .el-form-item__label {
          flex: none;
        }
        .el-input__prefix {
          display: none;
        }
      }
    }
    .el-tabs__header {
      display: flex;
      justify-content: center;
      margin-bottom: 30px;
      .el-tabs__nav-wrap {
        padding: 0;
        margin: 0;
        &:after {
          content: none;
        }
        .el-tabs__nav-scroll {
          .el-tabs__nav {
            background: #f5f5f5;
            border-radius: 25px;
            color: #6c6c6c;
            font-size: 14px;
            margin: 0 auto;
            .el-tabs__active-bar {
              width: 120px !important;
              margin-left: -16px;
              height: 38px;
              border-radius: 25px;
              background: #5478fb;
            }
            .el-tabs__item {
              z-index: 2;
              margin: 0;
              width: 120px;
              height: 38px;
              line-height: 38px;
              background: transparent;
              border: none;
              &.is-active {
                color: #ffffff;
              }
              span {
                margin: 0 auto;
              }
            }
          }
        }
      }
    }
  }
  .clean-property-box {
    padding: 30px 20px 0 20px;
    .el-tabs {
      height: 100%;
      .el-tabs__content {
        height: calc(100% - 104px);
        .custom-property-search {
          margin-bottom: 92px;
          height: 32px;

          .el-form {
            height: 32px;
          }
        }
        button {
          height: 32px;
          line-height: 32px;
        }
      }
      .el-tab-pane {
        height: 100%;
      }
    }
    .el-form {
      display: flex;
      align-items: center;
      .el-form-item {
        display: flex;
        align-items: center;
        .el-form-item__label {
          flex: none;
        }
        .el-input__prefix {
          display: none;
        }
      }
    }
    .el-tabs__header {
      display: flex;
      justify-content: center;
      margin-bottom: 92px;
      .el-tabs__nav-wrap {
        padding: 0;
        margin: 0;
        &:after {
          content: none;
        }
        .el-tabs__nav-scroll {
          .el-tabs__nav {
            background: #f5f5f5;
            border-radius: 25px;
            color: #6c6c6c;
            font-size: 14px;
            margin: 0 auto;
            .el-tabs__active-bar {
              width: 120px !important;
              margin-left: -16px;
              height: 38px;
              border-radius: 25px;
              background: #5478fb;
            }
            .el-tabs__item {
              z-index: 2;
              margin: 0;
              width: 120px;
              height: 38px;
              line-height: 38px;
              background: transparent;
              border: none;
              &.is-active {
                color: #ffffff;
              }
              span {
                margin: 0 auto;
              }
            }
          }
        }
      }
    }
  }
}
.model-box {
  .el-tabs--border-card > .el-tabs__content {
    padding: 0;
  }
  .el-tabs--border-card > .el-tabs__header .el-tabs__item {
    color: #555;
  }
}
.child-menu .el-submenu__title {
  position: absolute;
  left: 0;
  height: 40px;
  line-height: 40px;
  padding-left: 0 !important;
  padding-right: 0 !important;
  background: #2b314b !important;
  width: 100%;
  &:hover {
    background: #5274ff !important;
    color: #ffffff !important;
  }
}
.child-menu .el-menu--inline {
  margin-top: 40px;
}
</style>
<style lang="less" scoped>
@import "../../style/base.less";
.el-tabs.el-tabs--card.el-tabs--top {
  flex: 1 1 0%;
  display: flex !important;
  flex-direction: column !important;
}
.el-tabs.el-tabs--card.el-tabs--top .el-tabs__content {
  display: flex !important;
  flex: 1 !important;
}
.el-tabs--card > .el-tabs__header .el-tabs__item.is-active {
  background: #ffffff !important;
}
.el-tab-pane {
  height: 100%;
  display: flex;
}
.el-menu {
  width: 100%;
}
.el-menu-vertical-demo:not(.el-menu--collapse) {
  width: 100%;
}
.main {
  display: flex;
  flex: 1;
  width: 100%;
  height: calc(100vh - 88px);
  flex-direction: row;
  .search-section {
    height: 100%;
    background: #2b314b;
    .search-wrapper {
      height: 84px;
      background: #fff;
      .search-box {
        position: relative;
        height: 84px;
        padding: 10px 20px 10px 12px;
        border-bottom: 2px solid #fff;
        background: #2b3d53;
        color: #fff;
        .search-company {
          display: -webkit-box;
          width: 84px;
          line-height: 19px;
          overflow: hidden;
          text-overflow: ellipsis;
          line-clamp: 2;
          -webkit-line-clamp: 2;
          word-wrap: break-word;
          -webkit-box-orient: vertical;
          font-size: 14px;
          cursor: pointer;
        }
        .search-switch {
          position: absolute;
          top: 34px;
          right: 10px;
          font-size: 14px;
          color: #5478fb;
          cursor: pointer;
        }
        .search-period {
          height: 26px;
          line-height: 16px;
          padding-top: 10px;
          font-size: 12px;
        }
        .search-modal {
          position: absolute;
          top: 20px;
          left: 150px;
          width: 240px;
          height: 185px;
          padding: 0 16px;
          background: #2b3d53;
          border-radius: 4px;
          z-index: 9999;
          .left-arrow {
            position: absolute;
            top: 18px;
            left: -10px;
            width: 5px;
            height: 5px;
            border: 5px solid transparent;
            border-right: 5px solid #2b3d53;
          }
        }
      }
    }
    .search-text {
      width: 100%;
      height: 30px;
      padding: 0 8px;
      border: 1px solid #fff;
      border-radius: 4px;
      background: #fff url(../../image/search-icon.png) no-repeat 184px center;
      background-size: 16px;
      outline: none;
      &:focus {
        outline: none;
      }
      &:hover {
        outline: none;
      }
    }
    .search-panel {
      height: 128px;
      overflow-x: hidden;
      overflow-y: auto;
      &::-webkit-scrollbar {
        width: 5px;
        height: 5px;
        background-color: transparent;
      }
      &::-webkit-scrollbar-track {
        -webkit-box-shadow: inset 0 0 6px transparent;
        border-radius: 10px;
        background-color: transparent;
      }
      &::-webkit-scrollbar-thumb {
        border-radius: 10px;
        -webkit-box-shadow: inset 0 0 6px transparent;
        background-color: transparent;
      }
    }
    .search-item {
      height: 19px;
      line-height: 19px;
      margin-top: 13px;
      font-size: 14px;
      overflow: hidden;
      text-overflow: ellipsis;
      white-space: nowrap;
      &:hover {
        color: #5478fb;
        cursor: pointer;
      }
    }
  }

  .left-menu {
    width: 256px;
    height: calc(100% - 86px);
    display: flex;
    flex-direction: column;
    background: #2b314b;
    overflow: hidden;
    .el-menu {
      border: none;
    }
    .menuTitle {
      display: flex;
      height: 30px;
      align-items: center;
      padding: 10px 0 0 15px;
      background-color: #2b314b;
      .menuContent {
        .mixin-sc(12px, rgb(189, 195, 199));
      }
    }
    .split-line {
      height: 2px;
    }
    .menuMain {
      width: 100%;
      background-color: #2b314b !important;
      ul.first-menu-level {
        height: 100%;
        position: relative;
        li {
          background-color: #2b314b !important;
          padding-left: 0 !important;
          color: #ffffff;
        }
        li.menuList li {
          display: inline-block;
          padding: 0 0 0 26px !important;
          height: auto;
          &.first-menu {
            width: 100%;
            height: 40px;
            line-height: 40px;
          }
        }
        li.menuList.menu1 {
          li {
            padding-left: 46px !important;
          }
          .isSelectLi {
            background: #5274ff !important;
            color: #ffffff;
          }
        }
        li.menuList.menu1:hover {
          li {
            background: #5274ff !important;
            color: #ffffff;
          }
        }
      }
      ul .menuList.child-menu {
        width: 100%;
        overflow: hidden;
        .el-submenu {
          width: 100%;
          height: 40px;
          line-height: 40px;
          padding-left: 0 !important;
          .check-child {
            position: relative;
            width: 100%;
            height: 40px;
            line-height: 40px;
            margin: 0;
            padding-left: 0 !important;
            .el-menu-item {
              width: 100%;
              height: 40px;
              line-height: 40px;
              margin: 0;
              padding-left: 26px !important;
            }
          }
        }
      }
      ul .menuList li .menu-icon {
        display: inline-block;
        width: 16px;
        height: 16px;
        transition: all 1s ease-in-out;
        vertical-align: middle;
        position: absolute;
        top: 12px;
        left: 15px;
      }
      ul .menuList li .voucher-input {
        background: url("../../image/left_nav_icon.png") -20px -139px;
      }
      ul .menuList li .voucher-detail {
        background: url("../../image/left_nav_icon.png") -20px -180px;
      }
      ul .menuList li .total-account {
        background: url("../../image/left_nav_icon.png") -20px -302px;
      }
      ul .menuList li .subject {
        background: url("../../image/left_nav_icon.png") -20px -380px;
      }
      ul .menuList li .early-stage {
        background: url("../../image/left_nav_icon.png") -20px -420px;
      }
      ul .menuList li .action-log {
        background: url("../../image/left_nav_icon.png") -20px -658px;
      }
      ul .menuList li .voucher-export {
        background: url("../../image/left_nav_icon.png") -20px -500px;
      }
      ul .menuList li .item-type {
        background: url("../../image/left_nav_icon.png") -20px -540px;
      }
      ul .menuList li .fund-code {
        background: url("../../image/left_nav_icon.png") -20px -580px;
      }
      ul .menuList li .bank-icon {
        background: url("../../image/left_nav_icon.png") -20px -619px;
      }
      ul .menuList li .finance {
        background: url("../../image/left_nav_icon.png") -20px -659px;
      }
      ul li.child-menu {
        &.childMenuBox {
          padding-bottom: 14px;
        }
        .menu2 {
          background-color: #5274ff !important;
          color: #ffffff;
        }
        .check-child li {
          &:hover {
            background-color: #5274ff !important;
            color: #ffffff;
          }
        }
        .secondMenus {
          display: inline-block;
          width: 100%;
          font-size: 14px !important;
          padding-left: 46px !important;
        }
        .secondMenu .menu-icon,
        .secondMenus .menu-icon {
          display: inline-block;
          width: 16px;
          height: 16px;
          transition: all 1s ease-in-out;
          vertical-align: middle;
          position: absolute;
          top: 12px;
          left: 15px;
        }
        .secondMenu .account-book,
        .secondMenus .account-book {
          background: url("../../image/left_nav_icon.png") -20px -220px;
        }
        .secondMenu .statement,
        .secondMenus .statement {
          background: url("../../image/left_nav_icon.png") -20px -260px;
        }
        .secondMenu .property,
        .secondMenus .property {
          background: url("../../image/left_nav_icon.png") -20px -99px;
        }
        .secondMenu .assist-account,
        .secondMenus .assist-account {
          background: url("../../image/left_nav_icon.png") -20px -460px;
        }
        .secondMenu .assist-account,
        .secondMenus .invoice {
          background: url("../../image/left_nav_icon.png") -20px -20px;
        }
        .secondMenu .assist-account,
        .secondMenus .fund {
          background: url("../../image/left_nav_icon.png") -20px -59px;
        }
        .smart-tax{
          background: url("../../image/left_nav_icon.png") -20px -619px;
        }
      }
    }
    &:hover {
      overflow-y: auto;
    }
    &::-webkit-scrollbar {
      width: 5px;
      height: 5px;
      background-color: #2b3d53;
    }
    &::-webkit-scrollbar-track {
      -webkit-box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3);
      border-radius: 10px;
      background-color: #2b3d53;
    }
    &::-webkit-scrollbar-thumb {
      border-radius: 10px;
      -webkit-box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3);
      background-color: #555;
    }
  }
  .content {
    display: flex;
    flex: 1;
    flex-direction: column;
    overflow: scroll;
    .scroll__main {
      display: flex;
      height: 100%;
    }
    // padding:20px 0 20px 20px;
    // margin-right:20px;
  }
}
</style>

```



### 2. Router

```js

```



devWebpackConfig.plugins.push(
        new FriendlyErrorsPlugin({
          compilationSuccessInfo: {
            messages: [`App runing at: `,`Local: http://localhost:${port}`,`Network: http://${require('ip').address()}:${port}`,],
            // messages: []
          },
          onErrors: config.dev.notifyOnErrors
            ? utils.createNotifierCallback()
            : undefined
        })
      );

Config.js
    autoOpenBrowser: false,

src/api/statement/statement.js
// ExpenseList(params) {
  //   // 费用统计表
  //   let result = Post('/expense/list', params)
  //   return result
  // }


DateYearChoose.vue

### 3. src/components/dateSelection/DateYearChoose.vue

```js
<template>
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
      yearArray: [],
      newDate: 0,
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
      datePicker: null
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
  },
  mounted() {
    if (this.date.start) {
      console.log(111);
      const start = new Date(this.date.start);
      let startYear = start.getFullYear();
      this.startYear = startYear;

      const end = new Date(this.date.end);
      let endYear = end.getFullYear();
      this.endYear = endYear

      let num = startYear % 10;
      for (let i = -num; i < 10 - num; i++) {
        this.yearArray.push(startYear + i)
      }
      this.activeYear = startYear
    }
  },
  watch: {
    $route(to, from) {
      console.log(123432);
    },
    // date: {
    //   immediate: true,
    //   handler: function(n) {
    //     console.log(n);
    //     if (n && n.start) {
    //       const start = new Date(n.start);
    //       let startYear = start.getFullYear();
    //       console.log(startYear);
    //       this.yearArray.push(startYear - 1)
    //       for (let i = 0; i < 11; i++) {
    //         this.yearArray.push(startYear + i)
    //       }
    //       let startMonth = start.getMonth() + 1;
    //       startMonth = startMonth > 9 ? startMonth : `0${startMonth}`;
    //       let endYear = new Date(n.end).getFullYear();
    //       // this.activeYear = startYear;
    //       this.startYear = startYear;
    //       this.endYear = endYear;
    //       // this.active = `${startYear}-${startMonth}-01`;
    //     }
    //   }
    // },
    currentDate: {
      immediate: true,
      handler: function(date) {
        if (date) {
          this.activeYear = date
        }
      }
    }
  },
  methods: {
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

formatResponseData

```js
src/views/EnterpriseCenter/KeepAccount/VoucherQuery/VoucherQuery.vue

.el-table__body-wrapper::-webkit-scrollbar{

  // width:0px;

}


this.$refs.mainTable


this.$nextTick(() => {
  this.$refs.mainTable.bodyWrapper.scrollTop = scrollHeight
})

凭证插入问题
src/views/AccountSet/Voucher.vue
this.$nextTick(() => {
        this.initVoucherInfo.number = this.$route.params.number;
      })

src/views/AccountSet/Voucher.vue
:1989
// 获取路由传来的凭证单号
        if(this.$route.params.type = 'insert') {
          this.voucherInfo.number = this.$route.params.number;
        }

:2156
async $route(to, from) {
      console.log(12121212212121);
      await this.initAccountSet(this.accountSet)
      this.$nextTick(() => {
        this.voucherInfo.number = this.$route.params.number;
      })
      this.$store.dispatch("common/getDigests");
    },


src/views/Invoice/IncomeAndOutput/scanner.vue


src/views/EnterpriseCenter/Subject/subpage/addSubject.vue


:2485
          this.$router.push({ name: "voucherInput", params: { isAdd: true }});

            style="width: 100%;resize: none; border: 1px solid #fff;"
.form-control:focus {
  outline: none !important;
}


src/views/EnterpriseCenter/AccountBook/DetailAccount/DetailAccount.vue
:889
        this.detailAccountInit(JSON.parse(sessionStorage.getItem('DetailAccountPageSearch')))

//记忆
src/views/EnterpriseCenter/AccountBook/DetailAccount/DetailAccount.vue
:292
tempTime: {
        start:null,
        end:null,
      }
:461
          // this.tempTime.end = 


```

### 4. // 修改版费用统计表

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
      <!-- <el-col :span="2"> -->
          <!-- <DateYearChoose
            ref="dateChoose"
            :date="timeInterval"
            :current-date="monthReportDate"
            @conDatePicker="selectDateGetList"
          /> -->
        <!-- </el-col> -->
      <el-col :span="24">
      <div class="table-main" style="height: 100%">
        <el-table
          ref="mainTable"
          class="periodTable"
          :data="filterTableDate"
          border
          highlight-current-row
          height="100%"
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
// import DateYearChoose from "@/components/dateSelection/dateYearChoose";
let page;
export default {
  props: {
    refreshPage: {
      type: String,
      default: ""
    }
  },
  components: {
    // DateYearChoose
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
      timeInterval: {},
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
voucherInfo.date
```



### 显示区分等级

```js
methods: {
  marginFn(val) {
      if (val.level === 1) {
        return "0px";
      }
      if (val.level === 2) {
        return "20px";
      }
      if (val.level === 3) {
        return "40px";
      }
      if (val.level === 4) {
        return "60px";
      }
      if (val.level === 5) {
        return "80px";
      }
    },
}

<template slot-scope="scope">
              <div>
                <span
                  :style="{
                    marginLeft: marginFn(scope.row),
                  }"
                  >{{ scope.row.code }}</span
                >
              </div>
            </template>

```

```js
        <el-button class="btn btn-main btn-import" @click="refreshBtn">刷新</el-button>

refreshBtn() {
      this.init();
    },
```



远赴人间惊鸿宴

一睹人间盛世颜

最是人间留不住

朱颜辞镜花辞树



### 系统默认挂的是应收账款

```js
src/views/Invoice/IncomeAndOutput/invoiceCom.vue
:389
                  <i
                      v-if="scope.row.isShowEditor"
                      class="el-icon-edit"
                      @click="editAccountCode(scope.row)"
                    ></i>
:2114

    editAccountCode(row) {
      console.log(row);
    },
      
      
      editAccountDialogVisible = false

            this.type = 'detail'

```



### 银行

```js
<template>
  <div class="main-subject bank-main-subject">
    <div class="enterBank_group" v-if="searchShow">
      <search-model
        :isFund="true"
        :isBank="true"
        :search-obj="searchParams"
        :periodObj="this.getPeriodObj"
        @search-event="searchCashList"
      />
    </div>

    <div class="main-top">
      <el-col :span="9">
        <el-form
          :label-position="labelPosition"
          :inline="true"
          :model="formTiem"
          ref="formTiem"
        >
          <el-form-item>
            <el-date-picker
              format="yyyy年MM期"
              v-model="formTiem.period"
              :picker-options="disabledDateObj"
              :editable="false"
              :clearable="false"
              value-format="yyyy-MM-dd"
              type="month"
              style="width:150px"
              placeholder="请选择"
              @change="getListBankList"
            ></el-date-picker>
          </el-form-item>
          <el-form-item class="bank-list-arr">
            <el-select @change="changeBankList" v-model="formTiem.bankId">
              <el-option
                v-for="item in bankListArr"
                :key="item.id"
                :label="item.name"
                :value="item.id"
              >
              </el-option>
              <div class="add-cost-type" @click="addNewBank()">
                <i class="el-icon-circle-plus-outline"></i> 新增银行账户
              </div>
            </el-select>
            <!-- bankList -->
            <!-- <el-input
              placeholder="发票代码/发票号码/购方单位/发票金额"
              v-model.trim="formTiem.manyConditions"
            ></el-input> -->
          </el-form-item>
          <el-form-item>
            <el-button
              class="btn-main btn-wide-padding btn__add"
              @click="packUp"
              >高级查询</el-button
            >
          </el-form-item>
        </el-form>
      </el-col>
      <el-col :span="6">
        <p style="line-height: 40px">
          {{ checkBankData.accountCode }}-{{ checkBankData.accountFullName }}-{{
            checkBankData.name == "全部" ? "" : checkBankData.name
          }}
        </p>
      </el-col>
      <el-col :span="9">
        <div class="right">
          <!-- <el-button
          class="btn-main btn-wide-padding btn__add"
          @click="addVoucherBtn"
          v-if="getTodoById('fund-bank/update')"
          ><img src="@/image/btn-add.png" alt="" />新增</el-button
        > -->
          <el-button class="btn__default invoiceBtn" @click.stop="bankAddFun"
            >新增</el-button
          >
          <el-button
            class="btn__default invoiceBtn"
            @click.stop="moreImportBtn"
            v-if="getTodoById('fund-bank/update')"
            >票据上传</el-button
          >
          <el-button
            class="btn__default"
            @click="voucherBtn"
            v-if="getTodoById('fund-bank/batch-generate-voucher')"
            >生成凭证</el-button
          >
          <!-- <el-button
          class="btn__default"
          @click="batchRemove"
          v-if="getTodoById('fund-bank/update')"
          :loading="loading"
          >批量删除</el-button
        > -->
          <!-- <el-button
          class="btn-main btn-wide-padding btn__add"
          @click="addVoucherBtn"
          v-if="getTodoById('fund-bank/update')"
          ><img src="@/image/btn-add.png" alt="" />新增</el-button
        > -->
          <!-- <el-button
          class="btn__default"
          @click="setSubjectBtn"
          :loading="loading"
          >批量设置科目</el-button
        > -->

          <el-button style="" class="btn__default" @click.stop="moreBtn"
            >更多<i class="triangle_down"></i
          ></el-button>
          <div
            class="moreMenu"
            v-if="moreMenuShow"
            v-clickoutside="handleCloseMoreBtn"
          >
            <span class="circle_top"></span>
            <ul>
              <!-- <li><span @click="batchSupplement">批量补充</span></li> -->
              <li><span @click="handleImport">导入</span></li>
              <li><span @click="reMatchSubject">重新匹配科目</span></li>
              <li>
                <span @click="submitFnExtractBankShow">提取科目匹配关系</span>
              </li>
              <li>
                <span @click="subjectMatchingRelationship"
                  >科目匹配关系维护</span
                >
              </li>
              <!-- <li><span @click="submitFnAbstractBankShow">摘要设置</span></li> -->
              <!-- <li><span @click="reMatchSubject">删除凭证</span></li> -->
              <li><span @click="delectAccountStatement">删除流水</span></li>
              <li><span @click="setSubjectBtn">批量设置科目</span></li>
              <!-- <li><span @click="batchRemove" v-if="getTodoById('fund-bank/update')">批量删除</span></li> -->
            </ul>
          </div>
          <div
            class="moreMenu moreImportMenu"
            v-if="moreImportMenu"
            v-clickoutside="handleCloseMoreBtn"
          >
            <span class="circle_top"></span>
            <ul>
              <li><span @click="scannerIsShow('sanner')">票据扫描</span></li>
              <li>
                <span
                  @click="bankUpload(true)"
                  v-if="getTodoById('fund-bank/update')"
                  >票据上传</span
                >
              </li>
            </ul>
          </div>
        </div>
      </el-col>
    </div>
    <div class="main-content customer-box">
      <div class="table-main last-cell-hack cash-box add-bank-box">
        <el-table
          border
          height="'100%'"
          ref="mainTable"
          class="periodTable"
          :data="dataList"
          :row-class-name="highlightClass"
          @select-all="selectAll"
          @selection-change="handleSelectionChange2"
          @cell-mouse-enter="tableCellMouseEnter"
          @cell-mouse-leave="tableCellMouseLeave"
          highlight-current-row
          show-summary
          :summary-method="getSummaries"
          emptyText="暂无数据"
        >
          <!-- @row-click="rowClick" -->
          <!-- @selection-change="handleSelectionChange2" -->
          <!-- @select="handleSelectionChange" -->
          <el-table-column width="40px" align="center" type="selection">
          </el-table-column>
          <!-- <el-table-columnisSelectable
            type="selection"
            align="center"
            width="40"
          >
          </el-table-columnisSelectable> -->
          <el-table-column align="center" min-width="150px" label="交易日期">
            <template slot-scope="scope">
              <span>{{ scope.row.fundDetails[0].invoiceDate }}</span>
            </template>
          </el-table-column>
          <el-table-column align="center" min-width="250px" label="对方户名">
            <template slot-scope="scope">
              <span>{{ scope.row.fundDetails[0].assistName }}</span>
            </template>
          </el-table-column>
          <el-table-column align="center" min-width="150px" label="交易摘要">
            <template slot-scope="scope">
              <span>{{ scope.row.fundDetails[0].bankDigest }}</span>
            </template>
          </el-table-column>
          <el-table-column align="center" min-width="100px" label="收款金额">
            <template slot-scope="scope">
              <span>{{ scope.row.fundDetails[0].amountProc }}</span>
            </template>
          </el-table-column>
          <el-table-column align="center" min-width="100px" label="付款金额">
            <template slot-scope="scope">
              <span>{{ scope.row.fundDetails[0].amountPay }}</span>
            </template>
          </el-table-column>
          <el-table-column min-width="200px" align="center" label="收支类型">
            <template slot-scope="scope">
              <!-- <el-cascader
                filterable
                :props="{
                  value: 'id',
                  label: 'name',
                  children: 'documentTypeByFunds',
                  emitPath: false
                }"
                :options="
                  scope.row.fundDetails[0].amountProc
                    ? businessTypes
                    : businessTypes1
                "
                :show-all-levels="false"
                v-model="scope.row.fundDetails[0].documentTypeId"
                @change="
                  handelDocumentType(
                    scope.row,
                    scope.row.fundDetails[0].documentTypeId,
                    scope.$index
                  )
                "
              >
              </el-cascader> -->
              <el-cascader
              ref="aaa"
                filterable
                :props="{
                  value: 'id',
                  label: 'name',
                  children: 'children',
                  emitPath: false
                }"
                :options="options
                "
                :show-all-levels="false"
                v-model="scope.row.fundDetails[0].documentTypeId"
                @visible-change="v => visiblechange(v,{ ref: 'aaa', label, label1, icon, arrow})"
                @change="
                  handelDocumentType(
                    scope.row,
                    scope.row.fundDetails[0].documentTypeId,
                    scope.$index
                  )
                "
              >
                <template slot-scope="{ node, data }">
                  <div style="display: flex;justify-content: space-between;">
                    <span>{{ data.name }}</span>
                    <span v-if="!node.isLeaf" style="font-size: 20px;" @click="adddocumentType(node, data, scope)">+</span>
                  </div>
                </template>
              </el-cascader>
              <!-- <el-select
                v-model="scope.row.fundDetails[0].documentTypeId"
                @change="
                  handelDocumentType(scope.row, scope.row.fundDetails[0].documentTypeId,scope.$index)
                "
              >
                <el-option
                  v-for="(item, index) in businessTypes"
                  :key="index"
                  :title="item.name"
                  :label="item.name"
                  :value="item.id"
                ></el-option>
              </el-select> -->
            </template>
          </el-table-column>
          <el-table-column min-width="200px" align="center" label="科目">
            <template slot-scope="scope">
              <el-select
                class="subjectElSelect"
                ref="subjectselect"
                v-model="scope.row.fundDetails[0].documentAccountId"
                filterable
                placeholder="请选择"
                popper-class="subjectSelect"
                @change="
                  handelSubject(
                    scope.row,
                    scope.row.fundDetails[0].documentAccountId
                  )
                "
              >
                <el-option
                  v-for="item in selfSubjectOptions"
                  :key="item.id"
                  :title="item.code + ' ' + item.fullName"
                  :label="item.code + ' ' + item.fullName"
                  :value="item.id"
                >
                  <span
                    class="digest-option"
                    @click="
                      showBorrow(
                        scope.row,
                        '',
                        scope.row.fundDetails[0].documentAccountId
                      )
                    "
                    >{{ item.code + " " + item.fullName }}</span
                  >
                </el-option>
                <div class="add-cost-type" @click="addNewSubject()">
                  <i class="el-icon-circle-plus-outline"></i> 新增科目
                </div>
              </el-select>
            </template>
          </el-table-column>
          <el-table-column align="center" min-width="100px" label="入账期间">
            <template slot-scope="scope">
              <span>{{ scope.row.period }}</span>
            </template>
          </el-table-column>
          <el-table-column align="center" min-width="80px" label="凭证号">
            <template slot-scope="scope">
              <!-- <span>{{ scope.row.incomeDirection }}</span> -->
              <el-button
                v-if="scope.row.voucherStatus === 1 && !scope.row.isComplete"
                type="text"
                size="small"
                @click.stop="enterVoucherInfo(scope.row)"
                >记-{{ scope.row.voucherNumber }}</el-button
              >
              <el-button
                v-else-if="
                  (scope.row.voucherStatus === 0 || scope.row.isCheck) &&
                    !scope.row.isComplete
                "
                type="text"
                size="small"
                @click.stop="showVoucher(scope.row)"
                >预览</el-button
              >
            </template>
          </el-table-column>
          <el-table-column align="center" min-width="100px" label="操作">
            <template slot-scope="scope" v-if="getTodoById('fund-bank/update')">
              <!-- <span
                class="ic-on38"
                v-if="
                  scope.row.addWay !== 0 &&
                  !scope.row.isEdit &&
                  getTodoById('fund-bank/update')
                "
                @click.stop="previewInvoice(scope.row)"
              >
              </span> -->
              <!-- <span
                class="icon"
                :class="addIconStatus(scope.row)"
                @click.stop="addVoucherDetail(scope.row)"
              ></span> -->
              <span
                class="icon"
                :class="editIconStatus(scope.row)"
                @click.stop="bankEditorFun(scope.row)"
              ></span>
              <span
                class="icon"
                :class="deleteIconStatus(scope.row)"
                @click.stop="delectAccountStatementRow(scope.row)"
              ></span>
            </template>
          </el-table-column>
        </el-table>
      </div>
      <div class="page-box" style="margin-bottom: 0 !important">
        <v-pagination
          :pageSize="pageObj.pageSize"
          :total="total"
          :currentPage="pageObj.pageNum"
          @change-page-size="changePageSize"
          @change-page="changePage"
        />
      </div>
    </div>
    <create-voucher
      v-if="voucherShow"
      :generateVoucher="generateVoucher"
      :tempDialogTitle="this.tempDialogTitle"
      :voucherType="4"
      :propertyType="false"
      :fundId="fundId"
      :voucherAddWay="voucherAddWay"
      @click-cancel-form="cancelCreateVoucher"
      @save-template-success="saveTempSuccess"
    />
    <removeDialog
      v-if="dialogVisible"
      v-loading="loading"
      :title="title"
      :mainText="mainText"
      :dialogVisible="dialogVisible"
      @click-cancel="clickCancel"
      @click-sure="clickSure"
    />
    <!-- 银行对账单 -->
    <bank-record
      v-if="recordStatus"
      :title="title"
      :bankStatementId="bankStatementId"
      @click-cancel="recordStatus = false"
      @click-sure="clickSure"
    />
    <!-- 导入银行对账单 -->
    <export-record
      v-if="exportStatus"
      :title="title"
      @click-cancel="exportStatus = false"
      @click-sure="exportSuccess"
    />
    <!-- 新增银行 -->
    <add-edit-model
      v-if="addEditShow"
      :title="title"
      :type="addBankType"
      :id="''"
      @submit-success="submitFn"
      @close-model="closeModel"
    />
    <!-- 新增往来单位 -->
    <add-company
      v-if="showAddCompany"
      :title="'新增企业'"
      :ntype="'voucherInput'"
      @add-company-success="addCompanySuccess"
      @close-modal="cancelAddCompany"
    />
    <!-- 批量补充 -->
    <batch-supplement
      v-if="showSupplement"
      :title="'补全信息'"
      :batchList="batchList"
      :batchNumber="importBatch"
      @submit-success="supplementSuccess"
      @close-modal="cancelSupplement"
    />
    <!--    票据上传-->
    <bill-manager
      v-if="billManagerShow"
      :title="title"
      :pageSource="pageSource"
      :periodDate="periodDate"
      :isBillUpload="isBillUpload"
      @close-model="invoiceUploadClose"
    />
    <!--    票据预览-->
    <!-- <bill-carousel
      v-if="billCarouselShow"
      :data="invoiceDetailData"
      @close-model="closeCarouselModel"
      @show-manager="showManager"
      @reload-list="getInvoiceList"
    /> -->
    <bill-carousel
      v-if="billCarouselShow"
      :data="invoiceDetailData"
      @close-model="closeCarouselModelBank"
      @show-manager="showManager"
      @reload-list="getInvoiceList"
    />
    <!--    票据导入 -->
    <bank-import
      v-if="bankImportShow"
      title="票据导入"
      @close-model="closBankInpportlModel"
    />
    <scanner
      v-if="scanCarouselShow"
      @close-model="closeScanModel"
      @file-arr-model="getFileArrFun"
      :uploadType="uploadType"
      :source="'bank'"
    />
    <!-- 新增银行 -->
    <add-edit-model
      v-if="addBankShow"
      :title="addBakTitle"
      :type="addBankType"
      :id="addBankId"
      @submit-success="submitFnBankShow"
      @close-model="closeModelBankShow"
    />
    <!-- 批量修改科目  -->
    <subjectSet
      v-if="showSubjectSet"
      :title="subjectSetTit"
      :dialogVisible="showSubjectSet"
      :ids="multipleSelectionVal"
      :source="subjectSetSource"
      @submit-success="addSubjectSetSuccess"
      @click-cancel-form="cancelAddSetSubject"
      @close-model="closeSubjectSetModel"
    />
    <!-- 提取科目匹配关系  -->
    <bank-extract-matching
      v-if="extractBankShow"
      :title="extractBakTitle"
      :type="extractBankType"
      :ids="extractBankId"
      @submit-success="submitFnExtractBank"
      @close-model="closeModelExtractBankShow"
    />
    <!-- 摘要设置  -->
    <bank-abstract-set
      v-if="abstractBankShow"
      :title="abstractBakTitle"
      :type="abstractBankType"
      :id="abstractBankId"
      @set-cancel="setAbstractCancel"
      @submit-success="submitFnAbstractBank"
      @close-model="closeModelAbstractBankShow"
    />
    <add-subject
      v-if="showSubject"
      :title="'新增科目'"
      :source="'invoice'"
      :dialogVisible="showSubject"
      @submit-success="cancelAddSubject"
      @click-cancel-form="cancelAddSubject"
    />
    <bank-add
      v-if="addBankAccountShow"
      :title="addBankAccountTitle"
      :type="addBankAccountType"
      :id="addBankAccountId"
      :bankId="listBankId"
      :data="addBankAccountData"
      @submit-success="submitFnBankAccountShow"
      @close-model="closeModelBankAccountShow"
    />
    <AddType
      v-if="addTypeShow"
      :title="'新增'"
      :info-data="infoData"
      @submit="submitFn"
      @close-model="closeModelAddType"
    ></AddType>
  </div>
</template>

<script>
//#region
import { mapGetters, mapMutations } from "vuex";
import bank from "@/api/bank/bank";
import found from "@/api/found/found";
import accountBook from "@/api/accountBook/accountBook";
import VoucherInputAjax from "@/api/VoucherInput/VoucherInput";
import itemTypeAxios from "@/api/ItemType";
import { message } from "@/components/Message/Message";
import removeDialog from "@/components/Dialog/Dialog";
import VPagination from "@/components/Pagination/Pagination";
import addCompany from "../../Invoice/IncomeAndOutput/addInvoice/children/addCompany";
import addEditModel from "@/views/EnterpriseCenter/bank/children/addEditModel";
import createVoucher from "@/views/EnterpriseCenter/CheckoutTest/endOfTerm/children/createVoucher"; // 生成凭证组件
import bankRecord from "./child/bankRecord";
import searchModel from "./child/search";
import exportRecord from "./child/exportRecord";
import batchSupplement from "./child/batchSupplement";
import bankImport from "./child/bankImport";
import bankExtractMatching from "./child/bankExtractMatching"; //提取科目匹配关系
import bankAbstractSet from "./child/bankAbstractSet"; //摘要设置
import { clickoutside, getLastDay } from "@/tools/tools";
import AmountUtil from "@/utils/AmountUtil";
import BillManager from "./../IncomeAndOutput/BillCollection/billManager";
import BillCarousel from "./../IncomeAndOutput/BillCollection/billCarousel";
import scanner from "@/views/Invoice/IncomeAndOutput/scanner.vue";
import subjectSet from "../../Invoice/IncomeAndOutput/PopUp/subjectSet";
import addSubject from "@/views/EnterpriseCenter/bank/children/addSubject";
import bankAdd from "./child/bankAdd.vue";
import AddType from "./child/AddType.vue"
//#endregion
let that;
export default {
  data() {
    return {
      infoData: null,
      addTypeShow: false,
      itemTypeObj: {
        income: {
          index: 0,
          name: '收入',
          abbreviation: 'SR'
        },
        purchase: {
          index: 1,
          name: '采购',
          abbreviation: 'CG'
        },
        cost: {
          index: 2,
          name: '费用',
          abbreviation: 'FY'
        },
        collection: {
          index: 3,
          name: '收款',
          abbreviation: 'SK'
        },
        payment: {
          index: 4,
          name: '付款',
          abbreviation: 'FK'
        },
        other: {
          index: 5,
          name: '其他',
          abbreviation: 'QT'
        }
      },
      options: [
        {
          id: 1236337,
          code: "SK002",
          name: "收取预收款",
        },
        {
            id: 1236338,
            code: "SK003",
            name: "应收账",
            children: [
              {
                id: 1236336,
                name: "应收账款二级",
              }
            ]
          },
        {
            id: 1236330,
            code: "SK003",
            name: "应付账",
            children: [
              {
                id: 1236339,
                name: "应付账款二级",
              }
            ]
          },
      ],
      periodDate: "",
      searchParams: {
        type: 0,
        period: "",
        minAmount: "",
        maxAmount: "",
        voucherStatus: "",
        pageSize: 50,
        pageNum: 1
      },
      fundId: "",
      voucherAddWay: "",
      billCarouselShow: false,
      billManagerShow: false,
      gatherShow: false,
      bankImportShow: false,
      pageSource: "",
      addVoucherFlag: false, // 查询期间不等于当前期间 点击新增要跳转至当前期间
      addVoucherSuccess: false, // 跳转至当前期间刷新列表后在新增一条数据
      initFlag: false,
      focusFlag: false,
      focusIndex: 0, // 获取焦点的索引
      hoverVoucherId: 0,
      deleteVoucherId: "",
      formatTableData: [],
      companyArray: [], // 往来单位
      businessTypeArray: [], // 业务类型
      businessTypeArray1: [], // 业务类型
      businessTypeArray2: [], // 业务类型
      getPeriodObj: null, // 获取当前期间返回的数据props子组件（筛选）
      voucherShow: false,
      tableData: null,
      tabClick: false,
      selections: [],
      multipleSelectionVal: [],
      selectedVoucherArr: [],
      generateVoucher: null,
      dialogVisible: false,
      periodContent: "",
      searchShow: false,
      pageObj: {
        pageSize: 50,
        pageNum: 1
      },
      total: 0,
      deleteItemObj: {}, // 删除明细缓存
      removeRowObj: null,
      removeType: 0, // 0: 删除单条凭证分录； 1: 删除单条凭证； 2： 批量删除
      disabledDateObj: {
        // disabledDate(time) {
        //   // return (
        //   //   time.getTime() <
        //   //     new Date(that.getPeriodObj.currentPeriod).getTime() -
        //   //       24 * 60 * 60 * 1000 ||
        //   //   time.getTime() >=
        //   //     new Date(that.getPeriodObj.currentPeriodLastDay).getTime()
        //   // );
        // }
      },
      bankList: [], // 银行options
      bankListArr: [], // 银行options
      checkBankData: {},
      amountData: "", // 合计金额
      recordStatus: false,
      exportStatus: false,
      bankStatementId: 0,
      invoiceObj: null,
      getInvoiceShow: false,
      showAddCompany: false,
      addCompanyIndex: 0,
      invoiceParams: {
        invoiceId: 0,
        type: 0
      },
      addEditShow: false,
      addBankType: "Add",
      loading: false,
      moreMenuShow: false,
      batchList: [],
      importBatch: "",
      showSupplement: false,
      assistList: [],
      inAssistList: [],
      outAssistList: [],
      specialBusinessTypes: [],
      shortContent: "short-content",
      longContent: "long-content",
      dataList: [],
      moreImportMenu: false,
      scanCarouselShow: false,
      businessTypes: [], //收支类型
      businessTypes1: [], //收支类型
      selfSubjectOptions: [], //科目
      arrSubject: [],
      labelPosition: "right",
      formTiem: {
        period: "",
        bankId: ""
      },
      addBankShow: false,
      addBakTitle: "",
      addBankType: "",
      addBankId: "",
      showSubjectSet: false,
      subjectSetSource: "",
      subjectSetTit: "",
      multipleSelectionArr: [],
      extractBankShow: false, //提取科目匹配关系
      extractBakTitle: "",
      extractBankType: "",
      extractBankId: "",
      abstractBankShow: false, //摘要设置
      abstractBakTitle: "",
      abstractBankType: "",
      abstractBankId: "",
      showSubject: false,
      addBankAccountShow: "",
      addBankAccountTitle: "",
      addBankAccountType: "",
      addBankAccountId: "",
      addBankAccountData: {},
      listBankId: "",
      sameType: true //复选框收支类型
    };
  },
  beforeCreate() {
    that = this;
  },
  created() {
    this.getInvoiceShow = false;
    this.invoiceObj = this.$route.params;
    if (this.invoiceObj.id) {
      this.getInvoiceShow = true;
      this.invoiceParams.invoiceId = this.invoiceObj.id;
      this.invoiceParams.type = this.invoiceObj.foundTypeId;
      this.getPeriod(true);
    } else {
      this.getPeriod(); // 获取期间axios
    }
    this.getAssistList();
    this.getCompany();
    this.getBusinessType();
    this.getBankList();
    this.getBusinessTypes();
    this.getBusinessTypes1();
    this.getList();
    this.getSubjectArr();
  },
  computed: {
    ...mapGetters(["getTodoById", "getOrder"])
  },
  directives: { clickoutside },
  methods: {
    visiblechange(visible,{ ref, label ="", label1 ="取消", icon ="", arrow = false}){
      if (visible) {
        //级联选择器
        const _ref = this.$refs[ref];
        const that = this
        //找到el-cascader-panel 内容容器
        let popper = _ref[e].$children[1].$el
        //如果展开时展开内容盒中有子元素,就添加确定按钮
        if (popper .children.length)f
        //创建一个btn按钮
        const el = document.createElement( 'span');
        const elclose = document.createElement('span');//名称
        el.className ='btn' ,
        elclose.className ='close-btn';
        el.style =`border-top: solid 1px #E4E7ED;
        padding: ; position:absolute;
        bottom:-30px;right:e;
        width: 50%;height: 30px;text-align: center;
        line-height: 30px;
        cursor: pointer;
        color: #fff;
        background-color: #20AOFF;
        border-color: #409eff;
        font-size: 12px;
        border-radius: 3px;`
        elclose.style =`border-top: solid 1px #E4E7ED:
        padding: o; position:absolute;
        bottom:-30px;right:50%;
        width: 50%;height: 30px;text-align: center;
        line-height: 30px;
        cursor: pointer;
        color: #8492A6;
        background-color: #EFF2F7;
        border-color : #EFF2F7;
        font-size: 12px;
        border-radius: 3px;`
        //按钮内容
        el.innerHTML =`${label}`
        elclose.innerHTML =`${label1}`//添加btn到内容容器中
        popper.appendChild(el);
        popper.appendChild(elclose);
        }
      },
    submitFn() {
      console.log('1');
      this.addTypeShow = false;
    },
    closeModelAddType() {
      this.addTypeShow = false;
    },
    adddocumentType(node, data,scope) {
      this.addTypeShow = true
      console.log( {...data, ...node,...scope});
      this.infoData = Object.assign({}, {...data, ...node,...scope})

    },
    ...mapMutations([
      "updateSubjectOptions",
      "updateSubjectBackOptions",
      "showSubjectSelect"
    ]),
    // 合计
    getSummaries(param) {
      const { columns, data } = param;
      const sums = [];
      this.noUse = data;
      columns.forEach((column, index) => {
        sums[index] = 0;
        if (index === 2) {
          sums[index] = "合计";
        } else if (index === 4) {
          let amountProc = 0;
          this.dataList.map(item => {
            amountProc += item.fundDetails[0].amountProc
              ? item.fundDetails[0].amountProc
              : 0;
          });
          sums[index] = amountProc;
          sums[index] = this.numberFormat(Number(sums[index]).toFixed(2));
        } else if (index === 5) {
          let amountPay = 0;
          this.dataList.map(item => {
            amountPay += item.fundDetails[0].amountPay
              ? item.fundDetails[0].amountPay
              : 0;
          });
          sums[index] = amountPay;
          sums[index] = this.numberFormat(Number(sums[index]).toFixed(2));
        } else {
          sums[index] = "";
        }
      });
      return sums;
    },
    // 千位符
    numberFormat(sums) {
      if (sums === 0) {
        sums = "";
      } else if (!isNaN(sums)) {
        if (sums % 1 === 0) {
          sums = `${sums}`;
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
    // 新增银行流水
    bankAddFun() {
      if (this.listBankId) {
        this.addBankAccountShow = true;
        this.addBankAccountTitle = "新增";
        this.addBankAccountType = "add";
        this.addBankAccountId = 0;
      } else {
        this.$message({
          message: "请在列表选择银行后在新增！",
          type: "error"
        });
        return false;
      }
    },
    // 编辑银行流水
    bankEditorFun(val) {
      if (!val.isCheck || val.isCostSettle || Number(val.voucherStatus) === 1) {
        return false;
      }
      this.addBankAccountShow = true;
      this.addBankAccountTitle = "编辑";
      this.addBankAccountType = "editor";
      this.addBankAccountId = val.id;
      this.addBankAccountData = val;
    },
    // 新增银行流水成功回调
    submitFnBankAccountShow() {
      this.addBankAccountShow = false;
      this.getListBankList();
    },
    // 新增银行流水失败回调
    closeModelBankAccountShow() {
      this.addBankAccountShow = false;
    },
    //提取科目匹配关系
    submitFnExtractBankShow() {
      if (this.multipleSelectionVal.length > 0) {
        this.extractBankShow = true;
        this.moreMenuShow = false;
        this.extractBakTitle = "科目匹配关系提取";
        this.extractBankType = "bank";
        this.extractBankId = this.multipleSelectionVal;
      } else {
        this.$message({
          message: "请选择数据！",
          type: "error"
        });
      }
    },
    submitFnExtractBank() {
      this.extractBankShow = false;
    },
    closeModelExtractBankShow() {
      this.extractBankShow = false;
    },
    //摘要设置
    submitFnAbstractBankShow() {
      this.abstractBankShow = true;
      this.moreMenuShow = false;
      this.abstractBakTitle = "生成凭证设置";
      this.abstractBankType = "bank";
    },
    setAbstractCancel() {
      this.abstractBankShow = false;
    },
    closeModelAbstractBankShow() {
      this.abstractBankShow = false;
    },
    submitFnAbstractBank() {
      this.abstractBankShow = false;
    },
    // 科目匹配关系维护
    subjectMatchingRelationship() {
      this.$router.push({ path: "/center/bankSubjectMatching" });
    },
    // 重新匹配科目
    reMatchSubject() {
      if (this.multipleSelectionVal.length > 0) {
        bank.afreshMatch({ ids: this.multipleSelectionVal }).then(response => {
          if (response.code === 200) {
            this.$message({
              message: response.message,
              type: "success"
            });
            this.getList();
          } else {
            this.$message({
              message: response.message,
              type: "error"
            });
          }
        });
      } else {
        this.$message({
          message: "请选择数据！",
          type: "error"
        });
      }
    },
    setAbstract() {
      console.log('摘要设置');
    },
    // 收入科目设置
    setSubjectBtn() {
      if (this.multipleSelectionVal.length > 0) {
        if (this.sameType) {
          // this.clearSelectionFn()
          this.subjectSetTit = "批量修改科目";
          this.showSubjectSet = true;
          this.subjectSetSource = "bankSubjectSet";
        } else {
          this.$message({
            message: "请选择相同的收支类型，批量设置科目！",
            type: "error"
          });
        }
      } else {
        this.$message({
          message: "请选择数据！",
          type: "error"
        });
      }
    },
    addSubjectSetSuccess(data) {
      this.showSubjectSet = false;
      this.getList();
    },
    cancelAddSetSubject() {
      this.showSubjectSet = false;
    },
    closeSubjectSetModel() {
      this.showSubjectSet = false;
      this.getList();
    },
    // 更多查询
    packUp() {
      this.searchShow = !this.searchShow;
    },
    // 搜索
    cashNumber(value) {
      this.periodForm.minAmount = AmountUtil.formatNumberInput(value, 1);
    },
    maxAmount(value) {
      this.periodForm.maxAmount = AmountUtil.formatNumberInput(value, 1);
    },
    // 新增银行账户
    addNewBank() {
      this.addEditShow = true;
      this.addBankId = "";
      this.addBankType = "Add";
      this.addBakTitle = "新增银行信息";
    },
    submitFnBankShow(data) {
      this.addEditShow = false;
      this.getBankList();
    },
    closeModelBankShow() {
      this.addEditShow = false;
    },
    getBusinessTypes() {
      // 业务类型
      itemTypeAxios.getItemTypeFunList({ type: 3 }).then(res => {
        if (res.code === 200) {
          this.businessTypes = res.data;
        }
      });
    },
    getBusinessTypes1() {
      // 业务类型
      itemTypeAxios.getItemTypeFunList({ type: 4 }).then(res => {
        if (res.code === 200) {
          this.businessTypes1 = res.data;
        }
      });
    },
    getSubjectArr() {
      // 科目
      VoucherInputAjax.QuerySubject().then(response => {
        if (response.code === 200) {
          this.arrSubject = response.data;
          this.selfSubjectOptions = response.data;
        }
      });
    },
    handelDocumentType(data, val, index) {
      //业务类型change
      let accountId = "",
        code = "",
        queryParam = {},
        documentTypeId = "";
      let parentData = { ...data.parent };
      //业务类型，accountId  科目，id
      let typeArr =
        this.dataList[index].fundDetails[0].budgetType == "3"
          ? this.businessTypes
          : this.businessTypes1;
      typeArr.map(item => {
        if (item.documentTypeByFunds && item.documentTypeByFunds.length) {
          item.documentTypeByFunds.map(itemFunds => {
            if (itemFunds.id == val) {
              accountId = itemFunds.accountId;
            }
          });
        } else {
          if (item.id == val) {
            accountId = item.accountId;
          }
        }
      });
      // 更改科目默认值
      this.dataList[index].fundDetails[0].documentAccountId = accountId;
      let query = {
        bankId: data.bankId,
        fundDetails: [
          {
            budgetType: data.fundDetails[0].budgetType + "",
            assistName: data.fundDetails[0].assistName,
            documentTypeId: data.fundDetails[0].documentTypeId,
            accountId: accountId,
            remark: data.fundDetails[0].remark,
            invoiceDate: data.fundDetails[0].invoiceDate,
            amount: data.fundDetails[0].amount,
            fundId: data.fundDetails[0].fundId,
            id: data.fundDetails[0].id
          }
        ],
        id: data.id,
        type: data.type
      };
      bank.updateFundFun(query).then(res => {
        this.loading = false;
        if (res.code === 200) {
          this.$message({
            message: "修改成功！",
            type: "success"
          });
          this.getList();
        } else if (res.code === 400) {
          this.$message({
            message: res.message,
            type: "error"
          });
        }
      });
    },
    handelSubject(data, id) {
      // this.adjustmentFactorsData = data;
      // this.isAdjustmenShow = true;
      // this.adjustmenTitle = "调整因素";
      // this.arrSubject.map(item => {
      //   if (item.id == id) {
      //     this.adjustmentFactorsData.codeM = item.code;
      //     this.adjustmentFactorsData.fullNameM = item.fullName;
      //   }
      // });
      let query = {
        bankId: data.bankId,
        fundDetails: [
          {
            budgetType: data.fundDetails[0].budgetType + "",
            assistName: data.fundDetails[0].assistName,
            documentTypeId: data.fundDetails[0].documentTypeId,
            accountId: id,
            remark: data.fundDetails[0].remark,
            invoiceDate: data.fundDetails[0].invoiceDate,
            amount: data.fundDetails[0].amount,
            fundId: data.fundDetails[0].fundId,
            id: data.fundDetails[0].id
          }
        ],
        id: data.id,
        type: data.type
      };
      bank.updateFundFun(query).then(res => {
        this.loading = false;
        if (res.code === 200) {
          this.$message({
            message: "修改成功！",
            type: "success"
          });
          this.getList();
        } else if (res.code === 400) {
          this.$message({
            message: res.message,
            type: "error"
          });
        }
      });
    },
    showBorrow(item, index, selectSubjectId) {
      // 科目失去焦点打开金额输入框或者打开核算
      setTimeout(() => {
        if (!item.isAssist && !item.isQuantity) {
        } else if (item.isAssist) {
          // 有辅助核算
          let assistId = sessionStorage.getItem("assistId");
          let assistCategoryId = Number(
            sessionStorage.getItem("assistCategoryId")
          );
          if (String(item.assistId) === String(assistId)) {
            this.$nextTick(() => {
              this.queryAssitList(assistCategoryId, 0, "", "assitSelect");
            });
          }
        }
      }, 200);
    },
    addNewSubject() {
      this.showSubject = true;
    },
    // 预览凭证
    previewInvoice(row) {
      let id = "",
        invoiceFile = "";
      this.dataList.map(item => {
        if (item.id == row.id) {
          id = item.fundDetails[0].id;
          invoiceFile = item.fundDetails[0].invoiceFile;
        }
      });
      // invoiceFile = this.getImgType(invoiceFile)

      let that = this;
      // 获取当前凭证下标
      that.billCarouselShow = true;
      that.invoiceDetailData = {
        modalTitle: "银行回单扫描件", // 标题
        modalPageType: "bank", // 页面类型
        id: id,
        url: invoiceFile,
        periodDate: that.searchParams.period.substring(0, 7)
      };
    },
    // tif格式转换
    //  getImgType (url){
    //   // if(/\.(tif)$/.test(url)){
    //    let urls = 'https://account-dev.oss-cn-beijing.aliyuncs.com/invoice/20210711/image001-001.tif'
    //     let u = ''
    //     Axios.get("/fund/getJpgBase?tifPath="+urls)
    //     .then((res) => {
    //       if (res.data.code == 200) {
    //         u = 'data:image/jpeg;base64,' + res.data.data
    //         return u
    //       }else {
    //         this.$message({
    //           message: res.message,
    //           type: "warning",
    //         });
    //       }
    //     });
    //   // }else{
    //   //   return url
    //   // }
    // },
    // 关闭凭证预览弹窗
    closeCarouselModelBank(a) {
      this.billCarouselShow = false;
      this.getList();
      this.getBankAmount();
    },
    // 关闭扫描仪导入弹框
    closeScanModel() {
      this.scanCarouselShow = false;
      this.getList();
      this.getBankAmount();
    },
    // 关闭票据导入弹窗
    closBankInpportlModel() {
      this.bankImportShow = false;
      this.getList();
      this.getBankAmount();
      this.getCompany();
    },
    invoiceUploadClose() {
      this.billManagerShow = false;
      this.getInvoiceList();
      this.getList();
    },
    showManager() {
      this.billCarouselShow = false;
      setTimeout(() => {
        this.billUpload();
      }, 500);
    },
    billUpload(isBillUpload) {
      this.periodDate = this.searchParams.period.substring(0, 7);
      this.gatherShow = false;
      this.billManagerShow = true;
      this.title = "银行回单票据管理";
      this.isBillUpload = isBillUpload || false;
      this.pageSource = "bank";
    },
    bankUpload() {
      this.bankImportShow = true;
      this.moreImportMenu = false;
    },
    moreBtn() {
      this.moreMenuShow = !this.moreMenuShow;
    },
    moreImportBtn() {
      this.moreImportMenu = !this.moreImportMenu;
    },
    batchSupplement() {
      if (this.selectedVoucherArr.length === 0) {
        message({
          message: "请选择需要补充的数据！",
          type: "error"
        });
        return;
      }
      this.batchList = [];
      this.selectedVoucherArr.forEach(item => {
        this.batchList.push(item.id);
      });
      this.showSupplement = true;
      this.clearSelectionFn();
    },
    handleCloseMoreBtn() {
      this.moreMenuShow = false;
      this.moreImportMenu = false;
    },
    handleAssistChange(id, index) {
      this.companyArray.forEach(item => {
        if (item.id === id) {
          let assistId = item.accountId > 0 ? id : "";
          this.resetDocument(index, item.assistType, assistId);
        }
      });
    },
    resetDocument(index, assistType, assistId) {
      let documentObj = this.filterDocument(assistType);
      // this.formatTableData[index].documentTypeId = documentObj.id;
      if (documentObj.id) {
        this.formatTableData[index].relevantAssistantId = this.filterAssist(
          documentObj,
          assistId
        );
      } else {
        this.formatTableData[index].relevantAssistantId = "";
      }
      // this.handleDocumentChange(documentObj.id, index);
    },
    filterDocument(assistType) {
      let documentObj = {
        id: "",
        isAssistCustomer: false,
        isAssistProvider: false
      };
      let itemName = assistType === 2 ? "收销售款" : "支付供应商货款";
      this.businessTypeArray.forEach(item => {
        if (item.name === itemName) {
          documentObj.id = item.id;
          documentObj.isAssistCustomer = item.isAssistCustomer;
          documentObj.isAssistProvider = item.isAssistProvider;
        }
      });
      return documentObj;
    },
    filterAssist(documentObj, assistId) {
      let id = "";
      if (documentObj.isAssistCustomer) {
        this.inAssistList.forEach(item => {
          if (item.id === assistId) {
            id = item.id;
          }
        });
      }
      if (documentObj.isAssistProvider) {
        this.outAssistList.forEach(item => {
          if (item.id === assistId) {
            id = item.id;
          }
        });
      }
      return id;
    },
    handleDocumentChange(id, index) {
      this.businessTypeArray.forEach(item => {
        if (item.id === id) {
          this.formatTableData[index].budgetType = item.type;
          this.formatTableData[index].incomeDirection = this.getDireaction(
            item.type
          );
          if (
            (this.formatTableData[index].relevantAssistantType === 3 &&
              id !== -1) ||
            (this.formatTableData[index].relevantAssistantType !== 3 &&
              id === -1)
          ) {
            this.formatTableData[index].relevantAssistantId = "";
          }
          this.formatTableData[index].relevantAssistantType =
            item.associatedType;
          if (id === -1) {
            this.formatTableData[index].relevantAssistantType = 3;
          }
          this.filterByType(id);
        }
      });
    },
    // 新增科目成功
    addAccountSuccess() {
      this.showAddSubject = false;
    },
    // 新增科目取消
    cancelAddSubject() {
      this.showAddSubject = false;
      this.showSubject = false;
    },
    addNewCompany(index) {
      this.showAddCompany = true;
      this.addCompanyIndex = index;
    },
    addCompanySuccess(data) {
      this.assistList = [];
      this.getCompany();
      this.getBusinessType();
      this.getAssistList();
      this.showAddCompany = false;
      setTimeout(() => {
        this.formatTableData[this.addCompanyIndex].assistId = data.id;
        this.resetDocument(this.addCompanyIndex, data.assistType, data.id);
        this.addCompanyIndex = 0;
      }, 100);
    },
    cancelAddCompany() {
      this.showAddCompany = false;
      this.addCompanyIndex = 0;
    },
    getAssistList() {
      bank.queryAssistList({ type: 1 }).then(res => {
        if (res.code === 200) {
          this.inAssistList = res.data;
          this.assistList = res.data;
        }
      });
      bank.queryAssistList({ type: 2 }).then(res => {
        if (res.code === 200) {
          this.outAssistList = res.data;
          this.assistList = this.assistList.concat(res.data);
        }
      });
    },
    filterByType(id) {
      let data = {};
      this.businessTypeArray.forEach(item => {
        if (item.id === id) {
          data = item;
        }
      });
      if (data.isAssistCustomer && data.isAssistProvider) {
        this.assistList = this.inAssistList.concat(this.outAssistList);
      } else if (data.isAssistCustomer && !data.isAssistProvider) {
        this.assistList = this.inAssistList;
      } else if (!data.isAssistCustomer && data.isAssistProvider) {
        this.assistList = this.outAssistList;
      } else {
        this.assistList = [];
      }
    },
    supplementSuccess() {
      this.getList();
      this.getBankAmount();
      this.showSupplement = false;
    },
    cancelSupplement() {
      this.getList();
      this.getAssistList();
      this.getBankAmount();
      this.showSupplement = false;
      this.batchList = [];
      this.clearSelectionFn();
    },
    getBankAmount() {
      // 获取合计金额接口
      found.getBankAmount(this.searchParams).then(res => {
        if (res.code === 200) {
          this.amountData = res.data;
        }
      });
    },
    getBankList() {
      // 获取银行options
      found.getBankList({ pageNum: 1, pageSize: 500 }).then(res => {
        if (res.code === 200) {
          let data = res.data;
          this.bankList = res.data;
          data.unshift({ id: "", name: "全部" });
          this.bankListArr = data;
        } else if (res.code === 400) {
          message({
            message: `${res.message}`,
            type: "error"
          });
        }
      });
    },
    getDateNow(){
      var d = new Date();
      var y = d.getFullYear()
      var m = d.getMonth() +1
      var r = d.getDate()
      var data = y + "-" + (m < 10 ? "0" + m : m) + "-" + (r < 10 ? "0" + r : r)
      return data
    },
    getPeriod(isfromInvoice) {
      // 获取期间接口数据
      accountBook.QueryAccountPeriod().then(res => {
        if (res.code === 200) {
          this.getPeriodObj = res.data;
          let end = this.getDateNow()
          this.disabledDateObj = {
            disabledDate(time) {
              return (
                time.getTime() < new Date(res.data.start).getTime() ||
                time.getTime() >= new Date(end).getTime()
              );
            }
          };
          if (this.initFlag || !this.tabClick) {
            this.searchParams.period = this.formatLastDate(
              res.data.currentPeriod
            );
            this.formTiem.period = this.formatLastDate(res.data.currentPeriod);
            this.getBankAmount();
            this.initFlag = false;
          } else {
            this.tabClick = false;
          }
          if (!this.canEdit() || this.addVoucherFlag) {
            // 沒有未保存的數據(可以刷新列表)
            if (isfromInvoice && this.getInvoiceShow) {
              this.getInvoiceList();
            } else if (isfromInvoice === undefined) {
              this.getList();
            }
          } else {
            // 有未保存的數據(不能刷新列表)
            if (this.initFlag) {
              this.periodContent = `${res.data.currentPeriod
                .slice(0, 7)
                .split("-")
                .join("年")}期`;
              this.initFlag = false;
            } else if (isfromInvoice && this.getInvoiceShow) {
              this.getInvoiceList();
            }
          }
        }
      });
    },
    interSearch(time = 5) {
      this.searchCashList(
        {
          bankId: this.formTiem.bankId,
          period: this.formTiem.period
        },
        "bank"
      );
      this.timer = setTimeout(() => {
        this.interSearch();
      }, time * 1000);
    },
    searchCashList(searchData, from) {
      // 搜索列表
      this.addVoucherFlag = false;
      this.searchParams = Object.assign({}, this.searchParams, searchData);
      this.getList();
      // this.getBankAmount();
      // this.searchShow = false;
    },
    changeCustomerBtn() {
      // 新增银行信息
      this.addEditShow = true;
      this.id = "";
      this.addBankType = "Add";
      this.title = "新增银行信息";
    },
    async submitFn(data) {
      // 新增银行信息callback
      if (data.id) {
        this.addEditShow = false;
        await this.getBankList();
        this.formatTableData[0].bankId = data.id;
      }
    },
    closeModel() {
      this.addEditShow = false;
    },
    getInvoiceList() {
      this.periodContent = `${this.searchParams.period
        .slice(0, 7)
        .split("-")
        .join("年")}期`;
      found.fromInvoice(this.invoiceParams).then(res => {
        if (res.code === 200) {
          if (res.data !== null) {
            this.tableData = res.data;
            this.formatResponseData();
            this.pageObj.pageNum = res.data.pageNum;
            this.total = res.data.total;
            this.focusFlag = true;
            if (this.addVoucherFlag === true) {
              // this.addVoucherFlag为true时，设置this.addVoucherSuccess为一个标识等待列表接口请求完之后再去向列表中追加一条凭证，然后在watch监听
              this.addVoucherSuccess = true;
            } else {
              this.addVoucherSuccess = false;
            }
          } else {
            this.formatTableData = [];
          }
        }
      });
    },
    changeBankList(val) {
      this.listBankId = val;
      this.bankListArr.map(item => {
        if (item.id == val) {
          this.checkBankData = item;
        }
      });
      this.getListBankList();
    },
    getListBankList() {
      let query = {
        pageNum: this.pageObj.pageNum,
        pageSize: this.pageObj.pageSize,
        period: this.formTiem.period,
        bankId: this.formTiem.bankId,
        type: 0
      };
      found.queryOtherCarshList(query).then(res => {
        if (res.code === 200) {
          if (res.data !== null) {
            this.tableData = res.data;
            this.dataList = res.data.list;
            this.formatResponseData();
            this.pageObj.pageNum = res.data.pageNum;
            this.total = res.data.total;
            if (this.addVoucherFlag === true) {
              // this.addVoucherFlag为true时，设置this.addVoucherSuccess为一个标识等待列表接口请求完之后再去向列表中追加一条凭证，然后在watch监听
              this.addVoucherSuccess = true;
            } else {
              this.addVoucherSuccess = false;
            }
          } else {
            this.formatTableData = [];
          }
        }
      });
    },
    getList() {
      // 列表axios
      this.periodContent = `${this.searchParams.period
        .slice(0, 7)
        .split("-")
        .join("年")}期`;
      let query = { ...this.searchParams };
      if (query.tradeDate) {
        query.startTradeDate = query.tradeDate[0];
        query.endTradeDate = query.tradeDate[1];
      }
      delete query.tradeDate;
      found.queryOtherCarshList(query).then(res => {
        if (res.code === 200) {
          if (res.data !== null) {
            this.tableData = res.data;
            this.dataList = res.data.list;
            this.dataList.map(item => {
              item.fundId = item.id;
            });
            this.formatResponseData();
            this.pageObj.pageNum = res.data.pageNum;
            this.total = res.data.total;
            if (this.addVoucherFlag === true) {
              // this.addVoucherFlag为true时，设置this.addVoucherSuccess为一个标识等待列表接口请求完之后再去向列表中追加一条凭证，然后在watch监听
              this.addVoucherSuccess = true;
            } else {
              this.addVoucherSuccess = false;
            }
          } else {
            this.formatTableData = [];
          }
        }
      });
    },
    formatResponseData() {
      this.formatTableData = [];
      this.clearSelectionFn();
      this.tableData.list.forEach((item, index) => {
        let isComplete = false;
        item.fundDetails.forEach(row => {
          if (row.assistId === 0) row.assistId = "";
          if (row.relevantAssistantId === 0) row.relevantAssistantId = "";
        });
        isComplete = item.fundDetails.some(row => {
          return (
            !row.documentTypeId ||
            !row.documentTypeId ||
            row.documentTypeId === "" ||
            row.documentTypeId === ""
          );
        });
        if (item.id === null) {
          item.isInvoice = true;
        }
        this.formatTableData.push({
          id: item.id,
          fundId: item.id,
          addWay: item.addWay,
          number: item.voucherNumber, // 凭证号
          bankName: item.bankName,
          bankId: item.bankId,
          bankStatementId: item.bankStatementId,
          flag: true, // 自定义标识，在显示凭证概述栏使用
          isComplete: isComplete,
          isEdit: item.isInvoice || false,
          isSelected: false,
          isCheck: item.isCheck,
          voucherId: item.voucherId,
          isCostSettle: item.isCostSettle, // 费用新增数据同步到现金  不能编辑、删除 返回值true false
          voucherStatus: item.voucherStatus
        });
        item.fundDetails.forEach((fundItem, index) => {
          if (fundItem.id === null) {
            fundItem.totalPrice = this.invoiceObj.totalPrice;
          }
          fundItem.addWay = item.addWay;
          fundItem.voucherStatus = item.voucherStatus;
          fundItem.index = index;
          fundItem.isCostSettle = item.isCostSettle;
          fundItem.isCheck = item.isInvoice || item.isCheck;
          fundItem.isEdit = item.isInvoice || false; // 添加是否可编辑的标识
          fundItem.isSelected = false; // 添加训中的标识
          fundItem.incomeDirection =
            fundItem.budgetType === 1
              ? "收入金额"
              : fundItem.budgetType === 2
              ? "支出金额"
              : "未定义"; //0  未定义   1 收入   2 支出
          fundItem.documentTypeId =
            fundItem.documentTypeId === 0
              ? null
              : this.invoiceObj.businessTypeName
              ? this.invoiceObj.businessTypeName
              : fundItem.documentTypeId;
          this.formatTableData.push(fundItem);
        });
      });
    },
    formatLastDate(date) {
      // 格式化日期
      let year = date.slice(0, 4);
      let month = date.slice(5, 7);
      let day = getLastDay(year, month);
      return year + "-" + month + "-" + day;
    },
    addVoucherBtn() {
      // 新增凭证按钮
      if (this.canEdit()) {
        message({
          message: "存在未保存的操作，请先保存！",
          type: "error"
        });
      } else {
        this.addVoucherSuccess = false;
        let currentPeriod = `${this.getPeriodObj.currentPeriod
          .slice(0, 7)
          .split("-")
          .join("年")}期`;
        if (this.periodContent !== currentPeriod) {
          // 查询期间不等于当前期间1、this.addVoucherFlag为true 2、刷新期间接口跳转至当前期间
          this.addVoucherFlag = true;
          this.getPeriod();
        } else {
          // 等于当前期间直接追加一条凭证
          this.addVoucherArray();
        }
      }
    },
    addVoucherArray() {
      // 新增一条凭证封装一个方法
      this.clearSelectionFn();
      let newVoucher = [
        {
          id: 0,
          number: 0,
          fundId: 0,
          flag: true,
          isEdit: true,
          isSelected: false,
          fundDetails: []
        },
        {
          id: 0,
          index: 0, // 默认，已确定
          isEdit: true,
          isCheck: true,
          voucherStatus: "",
          isSelected: false,
          fundId: 0, // 资金id
          invoiceDate: this.formatLastDate(this.searchParams.period), // 日期
          assistId: "", // 往来单位id
          assistName: "", // 往来单位name
          accountSetId: "", // 账套id
          documentTypeId: "", // 业务类型id
          documentTypeName: "", // 业务类型name
          amount: "",
          budgetType: "",
          incomeDirection: "",
          relevantAssistantId: "",
          relevantAssistantType: 1,
          remark: ""
        }
      ];
      this.formatTableData = newVoucher.concat(this.formatTableData);
      this.focusFlag = true;
      return this.formatTableData;
    },
    validFormData(formData) {
      for (let i = 0; i < formData.length; i++) {
        if (formData[i].documentTypeId === "") {
          return {
            validStatus: true,
            message: "请选择业务类型！"
          };
        } else if (
          formData[i].amount === "" ||
          Number(formData[i].amount) === 0
        ) {
          return {
            validStatus: true,
            message: "金额为空，请输入！"
          };
        }
      }
      return {
        validStatus: false,
        message: ""
      };
    },
    saveVoucher(row) {
      // 保存凭证
      let data = [];
      let addNewItemFlag = false;
      let requestData = {};
      data = this.formatTableData.filter(item => {
        if (item.fundId === row.fundId) {
          if (item.id === 0) {
            addNewItemFlag = true;
          }
          return item;
        }
      });
      requestData.type = 0;
      requestData.id = row.id;
      requestData.bankId = row.bankId;
      requestData.fundDetails = data.slice(1, data.length);
      requestData.fundDetails[0].relevantAssistantType =
        requestData.fundDetails[0].relevantAssistantType || 0;

      let validStatusObj = this.validFormData(requestData.fundDetails);
      if (validStatusObj.validStatus) {
        message({
          message: validStatusObj.message,
          type: "error"
        });
      } else {
        if (row.id === 0) {
          // 新增凭证保存 (如果整体的 ID 等于 0，则为新增凭证)
          this.getAddCash(requestData);
        } else if (row.id === null) {
          this.getInvoiceAdd(requestData, row.bankId);
        } else if (addNewItemFlag) {
          // 新增分录保存 (整体的 ID 不等于 0 ，但是有 fundId 等于 0 的明细行)
          this.getEditCash(requestData);
        } else {
          this.getEditCash(requestData);
        }
      }
    },
    getInvoiceAdd(requestData, id) {
      let params = {
        invoiceId: 0,
        type: 0,
        bankId: id,
        invoiceGenerateFundDetailFormList: []
      };
      params.invoiceId = this.invoiceObj.id;
      params.invoiceGenerateFundDetailFormList = requestData.fundDetails;
      found.fromInvoiceAdd(params).then(res => {
        if (res.code === 200) {
          message({
            message: "保存成功！",
            type: "success"
          });
          this.getList();
        } else if (res.code === 400) {
          message({
            message: res.message,
            type: "error"
          });
        }
      });
    },
    getAddCash(requestData) {
      found.addOtherCash(requestData).then(res => {
        if (res.code === 200) {
          message({
            message: "保存成功！",
            type: "success"
          });
          this.getBankAmount();
          this.getList();
        } else if (res.code === 400) {
          message({
            message: res.message,
            type: "error"
          });
        }
      });
    },
    getEditCash(requestData) {
      found.updateOtherCash(requestData).then(res => {
        if (res.code === 200) {
          message({
            message: "保存成功！",
            type: "success"
          });
          this.getBankAmount();
          this.getList();
        } else {
          message({
            message: res.message,
            type: "error"
          });
        }
      });
    },
    showVoucher(row) {
      if (this.canEdit()) {
        message({
          message: "存在未保存的操作，请先保存！",
          type: "error"
        });
      } else {
        this.removeRowObj = row;
        found.bankPreview({ id: row.id }).then(res => {
          if (res.code === 200) {
            VoucherInputAjax.QueryDigest().then(digestResponse => {
              if (digestResponse.code === 200) {
                this.generateVoucher = res.data;
              }
            });
            this.tempDialogTitle = "凭证预览";
            this.voucherShow = true;
            this.fundId = row.id;
            this.voucherAddWay = row.addWay;
          } else if (res.code === 400) {
            message({
              message: res.message,
              type: "error"
            });
          }
        });
      }
    },
    saveTempSuccess(data) {
      // 生成凭证提交数据
      let params = {
        id: this.removeRowObj.id,
        voucherId: data.id,
        type: 0
      };
      found.saveOtherCashVoucher(params).then(res => {
        if (res.code === 200) {
          this.getList();
          this.voucherShow = false;
        }
      });
    },
    addVoucherDetail(row) {
      // 新增单个凭证分录
      if (!row.isCheck || row.voucherStatus === 1 || row.isCostSettle) {
        return;
      }
      if (this.canEdit(row.fundId)) {
        message({
          message: "存在未保存的操作，请先保存！",
          type: "error"
        });
      } else {
        this.clearSelectionFn();
        let newIndex; // 在 formatTableData 中的索引
        let itemIndex = 1; // 分录索引
        this.formatTableData = this.formatTableData.map((item, index) => {
          if (item.fundId === row.fundId) {
            itemIndex++;
            if (item.flag === true) {
              // 将当前凭证的编辑按钮改为保存
              item.isEdit = true;
            }
            if (item.index === row.index) {
              newIndex = index;
            }
          }
          return item;
        });
        this.focusFlag = true;
        this.focusIndex = itemIndex;
        this.formatTableData.splice(newIndex + 1, 0, {
          index: itemIndex, // 默认，已确定
          isEdit: true,
          isSelected: false,
          invoiceDate: this.formatLastDate(this.searchParams.period), // 日期
          id: 0,
          fundId: row.fundId, // 资金id
          assistId: "", // 往来单位id
          assistName: "", // 往来单位name
          accountSetId: "", // 账套id
          documentTypeId: "", // 业务类型id
          documentTypeName: "", // 业务类型name
          amount: "",
          bankId: "",
          bankName: "",
          remark: "",
          budgetType: "",
          incomeDirection: "",
          relevantAssistantId: "",
          relevantAssistantType: 1,
          isCheck: true
        });
      }
    },
    addIconStatus(row) {
      // 添加 icon 状态
      if (!row.id || row.id === 0) {
        return "el-icon-circle-plus-outline";
      }
      if (!row.isCheck || row.isCostSettle || Number(row.voucherStatus) === 1) {
        return "icon-34";
      }
      return "el-icon-circle-plus-outline";
    },
    editIconStatus(row) {
      // 编辑 icon 状态
      if (!row.id || row.id === 0) {
        return "el-icon-edit";
      }
      if (!row.isCheck || row.isCostSettle || Number(row.voucherStatus) === 1) {
        return "icon-35";
      }
      return "el-icon-edit";
    },
    deleteIconStatus(row) {
      // 删除 icon 状态 isCostSettle:新增费用结算金额同步到现金的数据字段
      if (
        !row.isCheck ||
        row.isCostSettle ||
        (row.voucherStatus === 1 && !row.voucherId)
      ) {
        if ((row.flag && row.id === 0) || !row.id) {
          return "el-icon-delete";
        }
        return "icon-36";
      }
      return "el-icon-delete";
    },
    editVoucher(row) {
      // isCostSettle:新增费用结算金额同步到现金的数据字段
      if (!row.isCheck || row.voucherStatus === 1 || row.isCostSettle) {
        return;
      }
      // 编辑凭证
      if (this.canEdit(row.fundId)) {
        message({
          message: "存在未保存的操作，请先保存！",
          type: "error"
        });
      } else {
        this.formatTableData = this.formatTableData.map(item => {
          if (item.fundId === row.fundId) {
            item.isEdit = true;
          }
          return item;
        });
      }
    },
    editVoucherDetail(row) {
      // 编辑单个凭证分录 isCostSettle:新增费用结算金额同步到现金的数据字段
      if (!row.isCheck || row.voucherStatus === 1 || row.isCostSettle) {
        return;
      }
      if (this.canEdit(row.fundId)) {
        message({
          message: "存在未保存的操作，请先保存！",
          type: "error"
        });
      } else {
        this.clearSelectionFn();
        this.formatTableData = this.formatTableData.map((item, index) => {
          if (item.fundId === row.fundId) {
            if (item.flag === true || item.index === row.index) {
              item.isEdit = true;
            }
          }
          return item;
        });
      }
    },
    deleteModel(type, row) {
      this.removeType = type;
      this.dialogVisible = true;
      this.removeRowObj = row;
      if (row && Number(row.voucherStatus) === 1) {
        this.mainText = "所选项已生成凭证，您确定要删除吗？";
      } else {
        this.mainText = "删除后不可恢复，您确定要删除吗？";
      }
      this.deleteVoucherId =
        row && row.voucherStatus === 1 ? row && row.voucherId : "";
    },
    removeRow(row) {
      // 删除单个凭证的分录
      if (
        !row.isCheck ||
        row.isCostSettle ||
        (row.voucherStatus === 1 && !row.voucherId)
      ) {
        return;
      }
      // 结算方式跳转过来 删除分录 => 清空数据
      if (row.id === null || row.id === "") {
        row.amount = "";
        row.assistId = "";
        row.documentTypeId = "";
        row.id = "";
        return row;
      }
      if (this.canEdit(row.fundId)) {
        message({
          message: "存在未保存的操作，请先保存！",
          type: "error"
        });
      } else {
        this.removeType = 0;
        this.deleteItemObj.fundId = row.fundId;
        this.deleteItemObj.id = row.id;
        this.deleteItemObj.index = row.index;
        if (row.id !== 0) {
          this.title = "删除";
          this.deleteModel(0, row);
        } else {
          this.deleteDetailAxios([this.deleteItemObj.fundId]);
        }
      }
    },
    tableCellMouseEnter(row, column, cell, event) {
      this.hoverVoucherId = row.fundId;
    },
    tableCellMouseLeave(row, column, cell, event) {
      this.hoverVoucherId = 0;
    },
    highlightClass({ row, index }) {
      if (row.fundId === this.hoverVoucherId) {
        return "row-hover";
      } else if (this.multipleSelectionVal.indexOf(row.fundId) !== -1) {
        return "highlightClass";
      } else if (row.flag) {
        return "row-title";
      } else {
        return "";
      }
    },
    isSelectable(row, index) {
      if (!row.isCheck || row.voucherStatus === 1) {
        // return false;
        return true;
      } else {
        return true;
      }
    },
    deleteVoucher(row) {
      // 删除单条凭证
      if (row.fundId === null) {
        row.isCostSettle = true;
      }
      if (!row.isCheck || row.isCostSettle) {
        if ((row.flag && row.id === 0) || row.id === null) {
          this.clearSelectionFn();
          this.formatTableData = this.formatTableData.filter(item => {
            if (item.fundId !== 0 && item.fundId !== null) {
              return item;
            }
          });
        }
        return;
      }
      if (this.canEdit(row.fundId)) {
        message({
          message: "存在未保存的操作，请先保存！",
          type: "error"
        });
      } else {
        this.clearSelectionFn();
        this.title = "删除";
        this.deleteModel(1, row);
      }
    },
    batchRemove() {
      // 批量删除凭证
      if (this.multipleSelectionVal.length <= 0) {
        message({
          message: "请选择要删除的信息！",
          type: "error"
        });
      } else {
        this.title = "批量删除";
        this.mainText = "删除后不可恢复，您确定要删除吗？";
        this.deleteModel(2);
      }
    },
    deleteAxios(deleteData) {
      // 删除axios
      found.removeOtherCash({ ids: deleteData }).then(res => {
        this.loading = false;
        this.dialogVisible = false;
        if (res.code === 200) {
          message({
            message: "删除成功!",
            type: "success"
          });
          this.getBankAmount();
          this.getList();
          let voucherId = Number(sessionStorage.getItem("voucherId"));
          if (this.deleteVoucherId === voucherId) {
            let objTab = document.getElementsByClassName(
              "el-tabs__item is-top is-closable"
            );
            for (let i = 0; i < objTab.length; i++) {
              if (objTab[i].textContent === "凭证信息") {
                objTab[i].firstElementChild.click();
              }
            }
          }
          this.selections = [];
          this.selectedVoucherArr = [];
          this.dialogVisible = false;
        } else if (res.code === 400) {
          message({
            message: res.message,
            type: "error"
          });
        }
      });
    },
    deleteDetailAxios(deleteData) {
      let getNewVoucher = this.formatTableData.filter(item => {
        if (item.fundId === this.deleteItemObj.fundId) {
          return item;
        }
      });
      if (getNewVoucher.length === 2) {
        this.deleteAxios(deleteData);
      } else {
        this.formatTableData = this.formatTableData.filter(item => {
          if (
            !(
              item.fundId === this.deleteItemObj.fundId &&
              item.index === this.deleteItemObj.index
            )
          ) {
            return item;
          }
        });
      }
      this.loading = false;
      this.dialogVisible = false;
    },
    handleImport() {
      this.clearSelectionFn();
      this.title = "导入银行对账单";
      this.exportStatus = true;
    },
    exportSuccess(importBatch) {
      this.batchList = [];
      this.clearSelectionFn();
      this.exportStatus = false;
      this.showSupplement = true;
      this.importBatch = importBatch;
      this.getBankAmount();
      this.getCompany();
      this.getBusinessType();
      this.getAssistList();
      this.getList();
    },
    // 删除银行流水
    delectAccountStatementRow(val) {
      let fundIds = [val.id];
      this.deleteAxios(fundIds);
    },
    // 批量删除银行流水
    delectAccountStatement() {
      this.loading = true;
      let fundIds = [];
      let flage = false;
      //  let str = ''
      this.selectedVoucherArr.forEach(item => {
        fundIds.push(item.id);
        if (item.voucherStatus == "1") {
          flage = true;
          // str = str+','+item.voucherNumber
        }
      });
      fundIds = [...new Set(fundIds)];
      if (flage) {
        this.$confirm("存在生成凭证的数据, 是否继续删除?", "提示", {
          confirmButtonText: "确定",
          cancelButtonText: "取消",
          type: "warning"
        })
          .then(() => {
            this.deleteAxios(fundIds);
          })
          .catch(() => {
            this.$message({
              type: "info",
              message: "已取消删除！"
            });
          });
      } else {
        this.deleteAxios(fundIds);
      }
    },
    clickSure() {
      this.loading = true;
      if (this.removeType === 2) {
        // 批量删除数据
        let fundIds = [];
        this.selectedVoucherArr.forEach(item => {
          fundIds.push(item.fundId);
        });
        this.deleteAxios(fundIds);
      } else if (this.removeType === 1) {
        // 删除单个凭证数据
        this.deleteAxios([this.removeRowObj.id]);
      } else {
        // 删除单个凭证分录数据
        this.formatTableData = this.formatTableData.map(item => {
          if (item.fundId === this.deleteItemObj.fundId) {
            if (item.flag === true || item.index === this.deleteItemObj.index) {
              item.isEdit = true;
            }
          }
          return item;
        });
        this.deleteDetailAxios([this.deleteItemObj.fundId]);
      }
    },
    voucherBtn() {
      // 点击生成凭证按钮
      if (this.canEdit()) {
        message({
          message: "存在未保存的操作，请先保存！",
          type: "error"
        });
      } else {
        if (this.selectedVoucherArr.length <= 0) {
          message({
            message: "请选择要生成凭证的票据信息！",
            type: "error"
          });
        } else {
          let fundIds = [],
            isExist = false;
          this.selectedVoucherArr.forEach(item => {
            if (fundIds.includes(item.fundId)) {
            } else {
              fundIds.push(item.fundId);
            }
          });
          found.getBankVoucher({ ids: fundIds }).then(res => {
            if (res.code === 200) {
              message({
                message: "凭证生成成功!",
                type: "success"
              });
              this.getList();
              this.selections = [];
              this.selectedVoucherArr = [];
            } else if (res.code === 400) {
              message({
                message: res.message,
                type: "error"
              });
            }
          });
        }
      }
    },
    enterVoucherInfo(row) {
      if (this.canEdit()) {
        message({
          message: "存在未保存的操作，请先保存！",
          type: "error"
        });
      } else {
        sessionStorage.setItem("voucherId", row.voucherId);
        this.$router.push({
          name: "voucherInfo",
          params: { voucherId: row.voucherId, isEdit: true }
        });
      }
    },
    getDireaction(budgetType) {
      // 根据选择的业务类型，判断金额方向
      if (budgetType === 0 || budgetType === 3) {
        return "收入金额";
      } else if (budgetType === 1 || budgetType === 2 || budgetType === 4) {
        return "支出金额";
      } else {
        return "";
      }
    },
    spanMethod({ row, column, rowIndex, columnIndex }) {
      // 合并单元格
      if (columnIndex === 0) {
        if (row.flag === true) {
          return {
            rowspan: this.computRowSpan(row.id),
            colspan: 1
          };
        } else {
          return {
            rowspan: 0,
            colspan: 1
          };
        }
      } else if (row.flag === true && columnIndex === 1) {
        return [1, 7];
      } else if (
        row.flag === true &&
        (columnIndex === 2 ||
          columnIndex === 3 ||
          columnIndex === 4 ||
          columnIndex === 5 ||
          columnIndex === 6 ||
          columnIndex === 7)
      ) {
        return [1, 0];
      }
    },
    computRowSpan(id) {
      // 计算要合并几行
      let length = 0;
      this.formatTableData.forEach(item => {
        if (item.fundId === id) {
          length++;
        }
      });
      return length;
    },
    canEdit(id) {
      // 是否存在未保存的操作
      if (id !== undefined) {
        return (
          this.formatTableData.findIndex(item => {
            return item.fundId !== id && item.isEdit === true;
          }) > -1
        );
      } else {
        return (
          this.formatTableData.findIndex(item => {
            return item.isEdit === true;
          }) > -1
        );
      }
    },
    rowClick(row) {
      // 已结账、已生成凭证的不能被选中
      if (!row.isCheck || row.voucherStatus === 1) {
        return;
      }
      this.formatTableData.forEach(item => {
        if (item.fundId === row.fundId) {
          this.$refs.mainTable.toggleRowSelection(item);
        }
      });
      let selectedFlag = false;
      selectedFlag = this.selections.some(item => {
        return item.fundId === row.fundId;
      });
      if (selectedFlag) {
        row.isSelected = false;
        this.selections = this.selections.filter(item => {
          return item.fundId !== row.fundId;
        });
      } else {
        row.isSelected = true;
        this.selections.push(row);
      }
      this.selectedVoucherArr = this.selections;
      if (this.tableData.list.length <= this.selectedVoucherArr.length) {
        this.isAllSelected = true;
      }
    },
    selectAll(val) {
      // 全选复选框处理
      if (!this.isAllSelected) {
        val[0].isSelected = true;
        this.selectedVoucherArr.push(val[0]);
        for (let i = 1; i < val.length; i++) {
          val[i].isSelected = true;
          if (val[i - 1].fundId !== val[i].fundId) {
            this.selectedVoucherArr.push(val[i]);
          }
        }
        this.selections = this.selectedVoucherArr;
        this.isAllSelected = true;
      } else {
        this.selections = [];
        this.selectedVoucherArr = [];
        this.isAllSelected = false;
        this.formatTableData = this.formatTableData.map(item => {
          item.isSelected = false;
          return item;
        });
      }
    },
    handleSelectionChange(val, row) {
      // 复选框处理
      let curId = row.fundId;
      if (row.isSelected) {
        this.selectedVoucherArr = this.selectedVoucherArr.filter(item => {
          return item.fundId !== curId;
        });
        val.map(item => {
          if (item.fundId === curId) {
            this.$refs.mainTable.toggleRowSelection(item);
            return item;
          }
        });
        this.isAllSelected = false;
      } else {
        this.selectedVoucherArr.push(row);
        if (this.tableData.list.length <= this.selectedVoucherArr.length) {
          document.querySelector(".add-bank-box .el-checkbox").click();
        }
      }
      this.selections = this.selectedVoucherArr;
      row.isSelected = !row.isSelected;
    },
    handleSelectionChange2(val) {
      this.multipleSelectionVal = [];
      let type = [];
      this.selectedVoucherArr = val;
      if (val.length) {
        val.forEach(item => {
          type.push(item.fundDetails[0] && item.fundDetails[0].budgetType);
          this.multipleSelectionVal.push(item.id);
        });
      }
      let flag = type && type[0];
      let result = type.every(item => item === flag);
      this.sameType = result;
    },
    clearSelectionFn() {
      // this.formatTableData = this.formatTableData.map((item) => {
      //   item.isSelected = false;
      //   return item;
      // });
      // this.$refs.mainTable.clearSelection();
      // this.isAllSelected = false;
      // this.selections = [];
      // this.selectedVoucherArr = [];
    },
    getTableIndex() {
      return this.formatTableData.forEach((item, index) => {
        item.index = index;
      });
    },
    searchBtn() {
      this.searchShow = true;
    },
    getCompany() {
      // 往来单位axios
      found.getCompanyList().then(res => {
        if (res.code === 200) {
          this.companyArray = res.data;
        }
      });
    },
    getBusinessType() {
      // 业务类型axios 未定义
      bank.queryDocumentList({ type: 0 }).then(res => {
        if (res.code === 200) {
          this.businessTypeArray = res.data;
        }
      });
      // 收入
      bank.queryDocumentList({ type: 4 }).then(res => {
        if (res.code === 200) {
          this.businessTypeArray1 = res.data;
        }
      });
      // 支出
      bank.queryDocumentList({ type: 5 }).then(res => {
        if (res.code === 200) {
          this.businessTypeArray2 = res.data;
        }
      });
      bank.queryDocumentList({ type: 3 }).then(res => {
        if (res.code === 200) {
          this.specialBusinessTypes = res.data;
        }
      });
    },
    changePageSize(pageSize) {
      // 改变数量
      this.pageObj.pageSize = pageSize;
      this.pageObj.pageNum = 1;
      this.searchParams = Object.assign({}, this.searchParams, this.pageObj);
      this.getList();
    },
    changePage(pageSize, pageNumber) {
      // 页码
      this.pageObj.pageNum = pageNumber;
      this.searchParams = Object.assign({}, this.searchParams, this.pageObj);
      this.getList();
    },
    handleClose() {
      this.searchShow = !this.searchShow;
    },
    cancelCreateVoucher() {
      this.voucherShow = false;
    },
    clickCancel() {
      this.dialogVisible = false;
    },
    formatListMoney(data, row, value) {
      return AmountUtil.formatMoney(data, row, value);
    },
    validAmount(e, data, rowObjec) {
      let value = e.target.value;
      value = value.replace(/[^\d.]/g, "");
      value = value.replace(/\.{2,}/g, ".");
      let leftVal = "";
      let rightVal = "";
      let isHasPoint = false;
      if (value.includes(".")) {
        leftVal = value.split(".")[0];
        rightVal = value.split(".")[1];
        isHasPoint = true;
      } else {
        leftVal = value;
      }
      leftVal = leftVal
        .replace(/^(-)?(\d{13}).*$/, "$1$2")
        .replace(/-?/g, (i => m => (!i++ ? m : ""))(0)); // 整数部分12位
      rightVal = rightVal.replace(/^(\d{2}).*/, "$1"); // 小数部分
      if (isHasPoint) {
        value = `${leftVal}.${rightVal}`;
      } else {
        value = `${leftVal}${rightVal}`;
      }
      // 结算方式跳过来的金额值不能大于价税合计值
      if (rowObjec.id === null) {
        if (value > Number(rowObjec.totalPrice)) {
          value = rowObjec.totalPrice;
          this.formatTableData[data].amount = value;
        }
      }
      this.$nextTick(() => {
        this.formatTableData[data].amount = value;
      });
    },
    // 上传显示
    scannerIsShow(type) {
      this.uploadType = type;
      this.scanCarouselShow = true;
    },
    // 上传关闭
    closeCarouselModel(v) {
      this.scanCarouselShow = false;
    },
    // 获取上传图片列表
    getFileArrFun(f) {
      // this.total = f.length;
    }
  },
  watch: {
    // searchShow: {
    //   immediate: true,
    //   handler: function(n) {
    //     console.info("searchShow", n);
    //     if (n) {
    //       console.info("clearTimer", this.timer);
    //       clearTimeout(this.timer);
    //     } else {
    //       this.interSearch();
    //     }
    //   }
    // },
    $route(to, from) {
      if (to.name === "invoiceBank") {
        if (from.name === "checkoutTest" && to.query.currentPeriod) {
          this.periodContent = `${to.query.currentPeriod
            .slice(0, 7)
            .split("-")
            .join("年")}期`;
          this.searchParams.period = to.query.currentPeriod;
          this.getList();
        } else {
          this.getInvoiceShow = false;
          if (to.params.id) {
            this.invoiceObj = to.params;
            this.getPeriod(true);
          } else {
            this.getPeriod();
          }
          this.tabClick = true;
          this.getCompany();
          this.getBusinessType();
          this.getBankList();
          this.getBankAmount();
          this.getAssistList();
        }
      }
    },
    getOrder: function(val) {
      if (val.isEnd === "success") {
        this.initFlag = true;
        this.getPeriod();
      } else if (val.isFromInvoice === "success") {
        // 进销项结算方式跳转资金对应页面
        this.getInvoiceShow = true;
        this.invoiceParams.invoiceId = this.invoiceObj.id;
        this.invoiceParams.type = this.invoiceObj.foundTypeId;
        this.getPeriod(true);
      }
    },
    addVoucherSuccess: function(newVal, oldVal) {
      if (newVal !== oldVal && newVal) {
        // 查询期间不等于当前期间，新增一条凭证到当前期间
        this.addVoucherArray();
      }
    }
    // formatTableData(newVal, oldVal) {
    //   if (this.focusFlag) {
    //     this.$nextTick(() => {
    //       if (!newVal[0].bankId) {
    //         this.$refs["focusItem0"].focus();
    //       }
    //     });
    //     this.focusFlag = false;
    //   }
    // },
  },
  components: {
    AddType, // 新增收支类型
    BillManager,
    BillCarousel,
    addCompany,
    createVoucher,
    removeDialog,
    searchModel,
    VPagination,
    bankRecord,
    exportRecord,
    addEditModel,
    batchSupplement,
    bankImport,
    scanner,
    subjectSet,
    bankExtractMatching, //提取科目匹配关系
    bankAbstractSet, //摘要设置
    addSubject, //新增科目
    bankAdd //新增流水
  }
};
</script>

<style lang="less" scoped>
@import "../../../style/base.less";
.enterBank_group {
  margin-bottom: 20px;
  padding: 24px 16px 24px 25px;
  background: #fff;
  width: 100%;
  border-radius: 4px;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.08);
  display: grid;
  grid-template-rows: auto 1fr;
  grid-template-columns: 1fr;
}
.main-top {
  padding-left: 16px;
  display: flex;
  justify-content: flex-end;

  .headContent {
    margin-left: 20px;
    font-size: 13px;
    color: #555;
  }
  .moreMenu {
    background: #ffffff;
    border: 1px solid #eeeeee;
    box-shadow: 0 1px 4px 0 rgba(0, 0, 0, 0.15);
    border-radius: 3px;
    width: 148px;
    max-height: 222px;
    position: absolute;
    z-index: 9999;
    top: 51px;
    right: 15px;
    ul {
      padding: 13px 0;
      width: 100%;
      height: 100%;
      display: flex;
      flex-direction: column;
      justify-content: space-around;
      li {
        cursor: pointer;
        .mixin-sc(12px, #555555);
        padding-left: 12px;
        width: 100%;
        height: 17px;
        line-height: 17px;
        margin-bottom: 10px;
      }
      li:hover {
        color: #5478fb;
      }
    }
    .circle_top {
      position: absolute;
      z-index: 10000;
      right: 30px;
      top: -15px;
      display: inline-block;
      width: 0;
      height: 0;
      border-width: 8px;
      border-style: solid;
      border-color: transparent transparent #ffffff transparent;
    }
  }
  .moreImportMenu {
    top: 45px;
    right: 246px;
    z-index: 9;
  }
}
.cash-amount {
  width: 100%;
  height: 26px;
  line-height: 26px;
  padding: 0 10px;
  box-sizing: border-box;
  border: 1px solid #eeeeee;
  outline: none;
  font-size: 13px;
  font-weight: normal;
  color: #606266;
  &:hover {
    border-color: #c0c4cc;
  }
  &:focus {
    border-color: #5478fb;
  }
}
.main-content.customer-box {
  height: calc(100% - 210px) !important;
  flex-direction: column;
  .table-main {
    margin-bottom: 0;
    padding-bottom: 1px;
    height: calc(100% - 40px);
    .cost-group-title-wrapper {
      display: flex;
      text-align: left;
      .voucher-number {
        flex: 1;
      }
      .option-items {
        width: 150px;
        text-align: right;
      }
    }
  }
}
.icon {
  margin: 0 10px;
  display: inline-block;
  width: 14px;
  height: 14px;
  cursor: pointer;
  font-size: 18px;
}
.el-icon-delete:hover:before,
.el-icon-edit:hover:before,
.el-icon-circle-plus-outline:hover:before,
.el-icon-picture-outline:hover:before {
  color: #5478fb;
}
.short-content {
  float: left;
  width: 48%;
}
.long-content {
  width: 100%;
}
</style>
<style lang="less">
.cash-box .el-table-column--selection {
  position: relative;
}
.cash-box .el-table-column--selection.is-leaf .cell {
  position: relative;
  top: 0;
}
.cash-box .el-table-column--selection .cell {
  position: absolute;
  top: 5px;
}
.cash-box .el-date-editor.el-input,
.el-date-editor.el-input__inner {
  width: auto;
}
.cash-box .el-input__inner {
  height: 26px;
  line-height: 26px;
}
.cash-box .el-table th,
.cash-box .el-table td {
  padding: 0 0;
}
.add-cost-type {
  position: absolute;
  bottom: 0;
  right: 0;
  width: 100%;
  height: 34px;
  line-height: 34px;
  border-top: 1px solid #ccc;
  text-align: center;
  background: #f3f3f3;
  color: #555;
  cursor: pointer;
  &:hover {
    border: 1px solid #5478fb;
    border-radius: 4px;
    color: #5478fb;
  }
}
.cost-business-popper {
  position: relative;
  .el-scrollbar {
    position: relative;
    padding-bottom: 35px;
  }
}
.bankName {
  display: inline-block;
  margin-right: 132px;
  width: 290px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
.bankNames {
  display: inline-block;
  width: 70px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
.fund-popper {
  position: relative;
  max-width: 357px;
  .el-scrollbar {
    position: relative;
    padding-bottom: 35px;
    &.is-empty {
      padding-bottom: 0;
      overflow: visible;
      .add-name-type {
        bottom: -65px;
      }
    }
  }
}
.no-data {
  text-align: center;
  font-size: 14px;
  background: #fff !important;
  color: #999 !important;
  cursor: default;
  &:hover {
    background: #fff;
    cursor: default;
  }
}
.add-name-type {
  position: absolute;
  bottom: 0;
  right: 0;
  width: 100%;
  height: 34px;
  line-height: 34px;
  text-align: center;
  background: #f3f3f3;
  color: #555;
  cursor: pointer;
  &:hover {
    border: 1px solid #5478fb;
    border-radius: 4px;
    color: #5478fb;
  }
}
.el-select-dropdown__list {
  padding-bottom: 120px !important;
  height:200px !important;
  overflow-y: auto !important;
}
</style>
<style>
.el-select-dropdown__list {
  padding-bottom: 120px !important;
  height:200px !important;
  overflow-y: auto !important;
}
.bankDate .el-input__inner {
  padding-right: 0 !important;
}
.resetHeight .el-input__inner {
  height: 26px !important;
}
.bank-main-subject .el-form--inline .el-form-item {
  margin-right: 5px;
}
.bank-main-subject button.el-button.btn-wide-padding.btn__add {
  padding: 6px 10px;
}
.bank-list-arr {
  width: 120px;
}
.bank-list-arr .el-form-item__content {
  width: 100% !important;
}
.bank-main-subject .right {
  justify-content: flex-end;
}
.bank-main-subject.main-subject .main-top {
  padding: 10px 20px 0px;
  background: #fff;
}
.short-content {
  float: left;
  width: 48%;
}
.long-content {
  width: 100%;
}
</style>

```



## // 修改科目

```js
src/views/EnterpriseCenter/Subject/Subject.vue
:357
this.showAddSubject = false

:488
QuerySubjectDetail (val) { // 科目详情
      SubjectAjax.SubjectDetail(val).then(res => {
        console.log(res);
        if (res.code === 200) {
          this.dataForm = res.data
          this.showAddSubject = true
          if(res.data.isLeaf) {
            if(!res.data.hasBeginning && !res.data.hasVoucher) {
              this.dataForm = res.data
              this.showAddSubject = true
            }else {
              this.dataForm = res.data
              this.changeSubjectDialogShow = true
            }
          }else {
            this.dataForm = res.data
            this.changeSubjectDialogShow = true
          }
          // if(res.data.hasBeginning && res.data.hasVoucher) {
          //   this.dataForm = res.data
          //   this.showAddSubject = true
          // }else {
          //   this.dataForm = res.data
          //   this.changeSubjectDialogShow = true
          // }
        }
      }).catch(() => {
      })
    },
      
      src/views/EnterpriseCenter/Subject/subpage/addSubject.vue
:287
// 无下级科目、没有凭证、没有期初
        if (!this.dataProps.hasBeginning && !this.dataProps.hasVoucher) {
          this.addFlag = true
          this.form.assist = this.dataProps.isAssist
          this.form.quantity = this.dataProps.isQuantity
          this.form.unit = this.dataProps.unit
          this.editFlag = false
          this.quantityDisabled = false
          this.assistDisabled = false
        } else {
          this.editFlag = false
          this.quantityDisabled = true
          this.assistDisabled = true
          this.directionFlag = true
        }

:301
} else {
        this.addFlag = false
        this.editFlag = false
        this.allDisabled = false

        // this.addFlag = false
        // this.allDisabled = true
      }
```

```js

      options: [{
          value: 'zhinan1',
          label: '指南',
          children: [{
            value: 'shejiyuanze1',
            label: '设计原则',
            children: [{
              value: 'yizhi1',
              label: '一致'
            }, {
              value: 'fankui1',
              label: '反馈'
            },]
          }, {
            value: 'daohang1',
            label: '导航',
            children: [{
              value: 'cexiangdaohang1',
              label: '侧向导航'
            }, {
              value: 'dingbudaohang1',
              label: '顶部导航'
            }]
          }]
        },
        {
          value: 'zhinan2',
          label: '指南2',
          children: [{
            value: 'shejiyuanze2',
            label: '设计原则',
            children: [{
              value: 'yizhi2',
              label: '一致'
            }, {
              value: 'fankui2',
              label: '反馈'
            },]
          }, {
            value: 'daohang2',
            label: '导航',
            children: [{
              value: 'cexiangdaohang2',
              label: '侧向导航'
            }, {
              value: 'dingbudaohang2',
              label: '顶部导航'
            }]
          }]
        },
        {
          value: 'zhinan3',
          label: '指南3',
          children: [{
            value: 'shejiyuanze3',
            label: '设计原则',
            children: [{
              value: 'yizhi3',
              label: '一致'
            }, {
              value: 'fankui3',
              label: '反馈'
            },]
          }, {
            value: 'daohang3',
            label: '导航',
            children: [{
              value: 'cexiangdaohang3',
              label: '侧向导航'
            }, {
              value: 'dingbudaohang3',
              label: '顶部导航'
            }]
          }]
        },
      ],
```





```js
src/views/ClientVisit/kjVisit.vue
:212
<el-table-column width="120px" label="常用联系人" prop="accountContactName" />
        <el-table-column width="140px" label="常用联系人手机" prop="accountContactPhone" />
          
        this.addContact = true


          
          
          
        
        
```



```js
this.$store.dispatch("common/getAccountSet");
```



:647

:loading="submitFormLoading"

:979

submitFormLoading: false

:1484

​      this.submitFormLoading = true;

:1492

​            this.submitFormLoading = false;

Taxation

CustomerTaxation



科目勾稽关系接口

```js
 “主营业务成本-折旧”借方发生额+“管理费用—折旧”借方发生额+“销售费用-折旧”借方发生额=“累计折旧”贷方发生额
internal-audit-business-balance-period
 “所得税费用”科目借方发生额=“应交税费-所得税”贷方发生额
internal-audit-expenses-balance-period
 “损益科目-工资”借方发生额=“应付职工薪酬-工资”贷方发生额
internal-audit-wages-balance-period
 “损益科目-社保”借方发生额=“应付职工薪酬-社保”贷方发生额
internal-audit-socialSecurity-balance-period
 “损益科目-公积金”借方发生额=“应付职工薪酬-公积金”贷方发生额
internal-audit-accumulationFund-balance-period
 “税金及附加”科目借方发生额=“应交税费-城建税”+“应交税费-教育附加”+“应交税费-地方教育附加”+“应交税费-印花税”+“应交税费-房产税”+“应交税费-土地使用税”+“应交税费-车船税”贷方发生额合计
internal-audit-Taxes-balance-period


// 科目勾稽关系检查
  // “主营业务成本-折旧”借方发生额+“管理费用—折旧”借方发生额+“销售费用-折旧”借方发生额=“累计折旧”贷方发生额
  internalAuditBusinessBalancePeriod (params) {
    let result = Post(`/check/internal-audit-business-balance-period?date=${params}`)
    return result
  },
  // “所得税费用”科目借方发生额=“应交税费-所得税”贷方发生额
  internalAuditExpensesBalancePeriod (params) {
    let result = Post(`/check/internal-audit-expenses-balance-period?date=${params}`)
    return result
  },
  // “损益科目-工资”借方发生额=“应付职工薪酬-工资”贷方发生额
  internalAuditWagesBalancePeriod (params) {
    let result = Post(`/check/internal-audit-wages-balance-period?date=${params}`)
    return result
  },
  // “损益科目-社保”借方发生额=“应付职工薪酬-社保”贷方发生额
  internalAuditSocialSecurityBalancePeriod (params) {
    let result = Post(`/check/internal-audit-socialSecurity-balance-period?date=${params}`)
    return result
  },
  // “损益科目-公积金”借方发生额=“应付职工薪酬-公积金”贷方发生额
  internalAuditAccumulationFundBalancePeriod (params) {
    let result = Post(`/check/internal-audit-accumulationFund-balance-period?date=${params}`)
    return result
  },
  // “税金及附加”科目借方发生额=“应交税费-城建税”+“应交税费-教育附加”+“应交税费-地方教育附加”+“应交税费-印花税”+“应交税费-房产税”+“应交税费-土地使用税”+“应交税费-车船税”贷方发生额合计
  internalAuditTaxesBalancePeriod (params) {
    let result = Post(`/check/internal-audit-Taxes-balance-period?date=${params}`)
    return result
  },
    
    
    
internalAuditCheckVoucher(type) {
      let res = [
        {message: "无赤字", level: 1, type: 11, accountList: []},
        {message: "无赤字", level: 2, type: 12, accountList: []},
        {message: "无赤字", level: 3, type: 13, accountList: []},
        {message: "无赤字", level: 1, type: 14, accountList: []},
        {message: "固定资产-累计折旧≥0", level: 1, type: 15, accountList: []},
        {message: "无赤字", level: 1, type: 16, accountList: []},
      ]
      // checkoutApi.internalAuditCheck(this.checkDateItem).then(res => {

      // })
      setTimeout(() => {
        for (let index = 0; index < this.internalAuditCheckList.length; index++) {
          this.internalAuditCheckList[index].message = res[index].message;
          this.internalAuditCheckList[index].level = res[index].level;
          this.internalAuditCheckList[index].accountList = res[ index ].accountList;
          this.internalAuditCheckList[index].iconClass = this.generageIconClass( res[index].level );
          this.internalAuditCheckList[index].msgClass = this.msgClass( res[index].level );
        }

        this.internalAuditCheckList.forEach(item => {
          if (item.level !== 1) {
            this.numsSeven++;
          }
        });
      }, 300)
```

```js
function formatDate(date) {
  let time = new Date(date);
  let y = time.getFullYear();
  let m = time.getMonth() + 1;
  let d = time.getDate();
  return `${y}-${m}-${d}`
}

```



```js

// xtl_fapiaokemuxuigai
发票批量修改结算科目
进项发票和销项发票修改结算科目 应收账款修改哪里





```













2023030015

```js
this.$router.push({
        path: `/order/orderManageDetails/${data.orderId}`,
        // query: {id: data.orderId},
      });
```



```js
white-space: normal;
min-width: 50px;
overflow: hidden;
text-overflow: ellipsis;
display: -webkit-box;
-webkit-box-orient: vertical;
-webkit-line-clamp: 1;
word-break: break-all;
```



 ```js
 let executeTimeEnd = this.formItem.executeTimeEnd.split(' ')[0];
         this.$set(this.formItem, 'executeTimeEnd', `${executeTimeEnd} 23:59:59`);
 ```



```js
 <el-table-column
        v-if="columns[8].visible"
        label="掉保天数"
        align="left"
        prop="datPublicIn"
      >
        <template slot-scope="scope">
          {{ scope.row.datPublicIn | getDiffDate }}
        </template>
      </el-table-column>


getBusinessTypes()
getBusinessTypes1()

filters:{
    getDiffDate(date) {
      let diffDate = new Date(date) - new Date();
      let iDays  =  parseInt(Math.abs(diffDate) / 1000 / 60 / 60 / 24 );
      return iDays + 1 + '天'
    }
  },
```





```js
// 从总账点科目编码跳到明细账，修改搜索的sessionStorage
src/views/EnterpriseCenter/AccountBook/Ledger/Ledger.vue
:391
let query = JSON.parse(sessionStorage.getItem('DetailAccountPageSearch'))
      query = {...query, start: data.date, end: data.date, endAccountCode: data.accountCode, startAccountCode:data.accountCode };
      sessionStorage.setItem('DetailAccountPageSearch', JSON.stringify(query));
```







会计绩效 accountingPerformance

主管绩效 supervisorPerformance.vue

个人绩效 individualPerformance.vue

接口文件 ErpAccountingPerformance.js

```js
// /erp/ErpAccountPerformance/personPerformancelist   会计个人实时绩效列表接口  POST

// /erp/ErpAccountPerformance/deptPerformancelist   会计主管实时绩效列表接口  POST

// /erp/ErpAccountPerformance/export  会计实时绩效导出接口    POST   入参和列表查询的一样  多传一个    exportType   导出类型 1:主管绩效，2：个人绩效




会计历史绩效查询接口
/erp/ErpAccountPerformance/oldAccountPerformanceList入参和实时查询的入参一样   多传一个参数   oldAccountPermanceType  历史报表查询类型 1:主管绩效查询，2：个人绩效查询  

会计绩效生成报表接口
/erp/ErpAccountPerformance/generateReport入参和实时查询的入参一样   多传一个参数   generateType    生成报表类型 1:主管绩效生成，2：个人绩效 

生成会计历史绩效导出报表接口 
/erp/ErpAccountPerformance/exportOldAccountPerformance    入参和实时查询的入参一样   多传一个参数  exportType   导出类型 1:主管绩效，2：个人绩效


/system/user/listAccountByRole    记账会计下拉列表  GET

/system/dept/getFinanceDeptByPid   部门下拉列表  GET
```



```js
币种 xtl_waibibizhong
结转汇兑损益 xtl_waibi_jiezhuan
科目档案 xtl_waibi_kemudangan 未修改
科目余额表 xtl_waibi_kemuyue 
明细账 xtl_waibi_mingxizhang
期初余额
新增凭证


// 外币核算
currency
Currency

// 科目档案
src/views/EnterpriseCenter/Subject/subpage/addSubject.vue
添加外币核算
:100
<el-form-item label="默认币种" prop="unit">
                <el-select v-model="form.defaultCurrency" placeholder="请选择" :disabled="quantityDisabled">
                  <el-option
                    v-for="item in currencyList"
                    :key="item.name"
                    :label="item.name"
                    :value="item.code">
                  </el-option>
                </el-select>
              </el-form-item>

// 科目余额表
numberFlag
currencyFlag

// 明细账
```







```js
外勤管理	731		外勤管理-创建任务，资料提供不允许选择一级
文件地址：
src/views/components/newTask/index.vue
```

```js
dev-api/erp/TaskInfo/getCity?parentId=64
```



```js
外勤管理，任务分配修改部门名字段
src/views/fieldManagement/is_assign/index.vue
:34
deptName
```

```js
<el-form-item label="部门" prop="deptId">
        <Department v-model="formItem.deptId" type="treeselect" multiple />
      </el-form-item>
```





```js
<template>
  <div class="app-container">
    <div style="text-align: center; width: 200px">
      <el-upload
        style=""
        class="avatar-uploader"
        action=""
        :http-request="uploadURL"
        :show-file-list="false"
        :on-success="handleAvatarSuccess"
        :before-upload="handleBeforeUpload"
      >
        <img v-if="fileUrl" :src="fileUrl" class="avatar">
        <i v-else class="el-icon-plus avatar-uploader-icon" />
      </el-upload>
      <el-button v-if="fileUrl" type="danger" @click="clearFile">删除<i class="el-icon-delete" /></el-button>


      <el-upload
        list-type="picture-card"
        :http-request="uploadURL"
        :on-success="handleAvatarSuccess"
        :before-upload="handleBeforeUpload"
        :on-remove="handleRemove">
        <i class="el-icon-plus"></i>
      </el-upload>
    </div>

  </div>
</template>
<script>
import {client} from '@/utils/alioss';

export default {
  props: {
    fileType: {
      type: Number
    },
    fileUrlSource: {
      type: Array
    },
  },
  data() {
    return {
      fileName: '',
      fileUrl: '',
    };
  },
  created() {
    this.fileUrl = this.fileUrlSource;
  },
  watch: {
    fileUrlSource: {
          handler: function (newV) {
              if (newV) {
                  this.fileUrl = newV
              }
          },
          deep: true
      },
  },
  methods: {
    handleAvatarSuccess(res, file) {
      // this.imageUrl = URL.createObjectURL(file.raw);
    },
    handleRemove(file) {

    },

    // 上传文件之前
    handleBeforeUpload(file) {
      this.fileNameTxt = file.name.lastIndexOf('.') === -1 ? file.name : file.name.substr(0, file.name.lastIndexOf('.'));
      this.fileEndTxt = file.name.split('.').slice(-1)[0];
      const isJPEG = this.fileEndTxt === 'jpeg';
      const isJPG = this.fileEndTxt === 'jpg';
      const isPNG = this.fileEndTxt === 'png';
      const isWEBP = this.fileEndTxt === 'webp';
      const isGIF = this.fileEndTxt === 'gif';
      const isLt500K = file.size / 1024 / 1024 / 1024 / 1024 < 4;
      if (!isJPG && !isJPEG && !isPNG && !isWEBP && !isGIF) {
        this.$message.error('上传图片只能是 JPEG/JPG/PNG 格式!');
      }
      if (!isLt500K) {
        this.$message.error('单张图片大小不能超过 4mb!');
      }
      return (isJPEG || isJPG || isPNG || isWEBP || isGIF) && isLt500K;
    },
    uploadURL(file) {
      let date = new Date();
      let dateYear = date.getFullYear(); // 获取年
      let dateMonth = date.getMonth() + 1; // 获取月
      let dateDate = date.getDate(); // 获取当日
      dateMonth = dateMonth > 9 ? dateMonth : '0' + dateMonth;
      dateDate = dateDate > 9 ? dateDate : '0' + dateDate;
      var handleFileName = this.fileNameTxt + '-' + new Date().getTime() + '.' + this.fileEndTxt;
      let dir = 'file/upload/course/' + dateYear + dateMonth + dateDate + '/' + handleFileName;
      // 定义唯一的文件名，打印出来的uid其实就是时间戳
      client().multipartUpload(dir, file.file, {
        progress: function(percentage, cpt) {
          console.log('打印进度', percentage);
        }
      }).then((res) => {
        // 此处赋值，是相当于上传成功之后，手动拼接服务器地址和文件名
        this.fileName = handleFileName;
        console.log(this.fileUrl);
        this.fileUrl = 'http://crm-file-com.oss-cn-beijing.aliyuncs.com/' + dir;
        this.$emit('getFileUrl', this.fileType, this.fileUrl);
      });
    },
    clearFile() {
      this.fileName = '';
      this.fileUrl = '';
      this.$emit('getFileUrl', this.fileType, this.fileUrl);
    },

  }
};
</script>
<style>
.avatar-uploader .el-upload {
  border: 1px dashed #d9d9d9;
  border-radius: 6px;
  cursor: pointer;
  position: relative;
  overflow: hidden;
}
.avatar-uploader .el-upload:hover {
  border-color: #409EFF;
}
.avatar-uploader-icon {
  font-size: 28px;
  color: #8c939d;
  width: 178px;
  height: 178px;
  line-height: 178px;
  text-align: center;
}
.avatar {
  width: 178px;
  height: 178px;
  display: block;
}
</style>

```

```js
import {
  getArea,
} from '@/api/fieldManagement/field_address.js';
getArea({parentId: this.form.cityId}).then((res) => {
        if (res.code == 200) {
          console.log(res.data);
          this.areaOptions = res.data;
        }
      });
```

```js
// 获取工商信息 字段
```



| legal_name | 法人      |
| ---------- | --------- |
| provinceId | 省id      |
| province   | 省        |
| cityId     | 城市id    |
| city       | 城市      |
| areaId     | 区id      |
| area       | 区        |
| townshipId | 街道/镇id |
| township   | 街道/镇   |
| dom        | 注册地址  |
| opscope    | 经营范围  |
| esdate     | 成立日期  |



120。300



```js
<el-form-item label="自定义服务类型" prop="vcServiceName" label-width="120px">
            <el-input v-model="queryParams.vcServiceName" placeholder="请输入自定义服务类型" clearable></el-input>
          </el-form-item>



<el-form-item label="需求人" prop="createdBy">
        <el-select
          v-model="formItem.createdBy"
          size="small"
          clearable
          filterable
          no-data-text="加载中"
          placeholder="请选择"
          @visible-change="getUserOption"
        >
          <el-option
            v-for="(it, i) in mateOptions"
            :key="i"
            :label="it.nickName"
            :value="it.userId"
          />
        </el-select>
      </el-form-item>
```





```js
// 合同
建筑资质合同新办12-20  jianzhuxinban
劳务建筑资质+安许 jianzhuAndAnxu
劳务建筑资质 newTwoJianzhu
委托股权转让合同（买执照）销售用 newshougou
```

  

```js
// 会计绩效跳转记账客户列表 请求传参删除分页
src/views/erp/Service/TaxControService.vue
:899
let queryParams = JSON.parse(JSON.stringify(this.queryParams));
delete queryParams.pageNum
delete queryParams.pageSize
getEnterpriseServiceList({sServiceMainIds: data, operateType: operateType, ...queryParams}).then(response => {
        this.list.list = response.rows;
        this.list.total = response.total;
        this.list.loading = false;
      });
```



​      <el-input v-model="contractPaymentParams.remark" class="textarea " type="textarea" placeholder="备注" maxlength="500"



```js

```

2023050435707

```js
orderData: {
      immediate: true,
      handler: function (n) {
        if (n) {
          this.orderDataMain = JSON.parse(JSON.stringify(n))
          this.orderDataMain.contractContent = Object.assign({}, {...this.orderDataMain.contractContent}, {...JSON.parse(n.contractContentStr)}); // 添加的一句
          this.orderDataMain.contractContent.contractDetailObject = this.orderDataMain.contractContent.contractDetailObject || {}
          this.orderDataMain.contractContent.firstContactPhone =this.orderDataMain.contractContent.firstContactPhone|| this.phoneNumber;
          this.orderDataMain.contractContent.secondParty = this.orderDataMain.contractContent.secondParty ||this.$store.state.user.userInfo.dept.deptName;
          this.orderDataMain.contractContent.secondContactName = this.orderDataMain.contractContent.secondContactName || this.$store.state.user.userInfo.nickName;
        }
      },
    },
;
// 

<el-table-column label="变更时间" prop="changeTime" align="center">
        <template slot-scope="scope">
          {{ scope.row.changeTime ? `${new Date(scope.row.changeTime).getFullYear()}-${new Date(scope.row.changeTime).getMonth() + 1}-${new Date(scope.row.changeTime).getDate()}` : ''}}
        </template>
      </el-table-column>


```



```js
// 业支 图片放大缩小
src/views/components/serve/informationCollection.vue
:614
pictureCardWidth: 500,
:960
closeNotify() {
      this.pictureCardWidth = 500;
      if(this.notify) this.notify.close();
    },
    handlePictureCardPreview1(file){
      this.dialogImageUrl = file.url;
      this.closeNotify();
      const h = this.$createElement;
      this.notify = this.$notify({
        dangerouslyUseHTMLString: true,
        message: h('div', null,
        [
          h('div',
            {
              style: {
                float: "right",
                userSelect: "none",
              }
            },
            [
              h('button', {
                style: {
                  display: "block",
                  width: "26px",
                  textAlign: "center",
                  cursor: "pointer",
                  border: 'none',
                  background: "#448ef7",
                  color: "#fff",
                  borderRadius: "5px",
                },
                on: {
                  click: this.enlarge,
                }
              }, '+'),
              h('button', {
                style: {
                  display: "block",
                  width: "26px",
                  textAlign: "center",
                  margin: "10px 0 0 0",
                  cursor: "pointer",
                  border: 'none',
                  background: "#448ef7",
                  color: "#fff",
                  borderRadius: "5px"
                },
                on: {
                  click: this.shrink
                }
              }, '-')
            ]
          ),
          h('img', {
            style: {
              width: this.pictureCardWidth + 'px'
            },
            ref:"pictureCardImg",
            attrs: {
              src: this.dialogImageUrl,
            },
          })
        ]),
        duration: 0,
      });
      this.dialogVisible = true;
    },
    enlarge() {
      if(this.pictureCardWidth > 1500) return;
      this.pictureCardWidth += 100;
      this.$refs.pictureCardImg.style.width = this.pictureCardWidth + 'px';
    },
    shrink() {
      if(this.pictureCardWidth < 400) return;
      this.pictureCardWidth -= 100;
      this.$refs.pictureCardImg.style.width = this.pictureCardWidth + 'px';
    },
```



```js
// 财务要求,此处页面的订单编号,只是单纯的查看详情,跟点击订单列表查看详情保持一致
    getOrderDetails(data) {
      this.row = data;
      this.$router.push({
        path: '/order/orderManageDetails',
        query: {
          id: data.orderId,
          isSource: 'orderList'
        },
      });
    };
 git checkout -b xtl_qizhaoduo
<el-form-item label="提单指定产品:" prop="selectProduct" label-width="300px">
              <!-- optionsProduct -->
              <el-table
                ref="multipleTable"
                :data="tableData"
                tooltip-effect="dark"
                style="width: 100%"
                @selection-change="handleSelectionChange">
                <el-table-column type="selection" width="55"></el-table-column>
                <el-table-column prop="name" label="姓名" width="120"> </el-table-column>
                <el-table-column
                  label="日期"
                  width="120">
                  <template slot-scope="scope">{{ scope.row.date }}</template>
                </el-table-column>
                <el-table-column prop="address" label="地址" show-overflow-tooltip> </el-table-column>
              </el-table>
              <!-- <el-select
                v-model="addForm.selectProduct"
                filterable
                placeholder="请选择"
                multiple
                style="width:100%"
                value-key="numProductId"
                @change="selectProductHandle">
                <el-option
                  v-for="(item,index) in optionsProduct"
                  :key="item.numProductId + index"
                  :label="item.vcServiceName"
                  :value="item" />
              </el-select> -->
            </el-form-item>
```



```js
xtl_dev2
// 修改银行、单据类型设置科目下拉框选项
accountBook.SubjectList()
import accountBook from "@/api/accountBook/accountBook";
:273
<!-- 1122 应收账款、2203 预收账款、1221 其他应收款、2202 应付账款、1123 预付账款、2241 其他应付款 -->
              <!-- ["1122", "2203", "1221", "2202", "1123", "2241"] -->
:277
                v-model="specialSubjectArr.includes(scope.row.fundDetails[0].documentType.accountCode) ? scope.row.fundDetails[0].documentType.accountId : scope.row.fundDetails[0].documentAccountId"

:632
      specialSubjectArr: ["1122", "2203", "1221", "2202", "1123", "2241"],

,
    specialSubject: {
      
    }

```









batchEditSettlement  // 进销项发票修改接口

### 发票汇总

```vue
<template>
    <el-dialog
      title="发票汇总"
      width="700px"
      class="dialog-main case-detail-box invoice-sum-box"
      :visible="true"
      :close-on-click-modal="false"
      :before-close="handleClose"
    >
      <div class="dialog-box-main case-detail">
          <div class="table-main">
                <el-table
                  ref="mainTable"
                  class="case-sum"
                  :data="detailData"
                  border
                  emptyText="暂无数据"
                  height="200px">
                  <el-table-column
                    min-width="24%"
                    align="left"
                    label="发票类型">
                    <template slot-scope="scope">
                        <span>增值税专用发票</span>
                    </template>
                  </el-table-column>
                  <el-table-column
                    min-width="15%"
                    align="center"
                    v-if="isPurchase"
                    label="抵扣状态">
                    <template slot-scope="scope">
                        <span>{{scope.row.isDeduction ? '已抵扣' : '不抵扣'}}</span>
                    </template>
                  </el-table-column>
                  <el-table-column
                    align="center"
                    min-width="15%"
                    prop="sumInvoice"
                    label="发票张数">
                  </el-table-column>
                  <el-table-column
                    align="right"
                    min-width="20%"
                    :formatter="formatMoney"
                    prop="totalAmount"
                    label="金额合计">
                  </el-table-column>
                  <el-table-column
                    align="right"
                    min-width="20%"
                    prop="totalTaxAmount"
                    :formatter="formatMoney"
                    label="税额合计">
                  </el-table-column>
                  <el-table-column
                    align="right"
                    min-width="20%"
                    prop="totalPrice"
                    :formatter="formatMoney"
                    label="价税合计">
                  </el-table-column>
                </el-table>
                <el-table
                  ref="mainTable2"
                  class="case-sum"
                  style="margin: 10px 0 0 0;"
                  :data="subjectDetailData"
                  border
                  emptyText="暂无数据"
                  height="220px"
                  v-if="!isPurchase"
                >
                  <el-table-column min-width="60%" align="center" prop="subjectName" label="科目" ></el-table-column>
                  <el-table-column align="right" min-width="40%" prop="price" :formatter="formatMoney" label="金额"></el-table-column>
                </el-table>
            </div>
            <div class="footer">
                <el-button @click="handleClose" class="btn-main btn-wide-padding">关闭</el-button>
            </div>
      </div>
    </el-dialog>
</template>

<script>
import invoice from '@/api/Invoice/invoice'
import AmountUtil from '@/utils/AmountUtil' // 公共js方法

export default {
  props: {
    isPurchase: {
      type: Boolean,
      default: false
    },
    period: {
      type: String,
      default: ''
    }
  },
  data () {
    return {
      detailData: [
        {
          type: null,
          isDeduction: true
        },
        {
          type: null,
          isDeduction: false
        }
      ],
      subjectDetailData: []
    }
  },
  created () {
    this.initList()
  },
  methods: {
    initList () {
      invoice.invoiceSum({isPurchase: this.isPurchase, period: this.period}).then(res => {
        if (res.code === 200) {
          if (res.data.length > 0) {
            this.detailData = res.data
          } else {
            if (!this.isPurchase) {
              this.detailData = []
              let arr = this.detailData[0]
              this.detailData.push(arr)
            }
          }
        }
      })
      if(!this.isPurchase) {
        // 发送销项发票科目明细请求
        this.subjectDetailData = [
          {
            subjectName: '123',
            price: 1234543
          },
          {
            subjectName: '123',
            price: 1234543
          },
          {
            subjectName: '123',
            price: 1234543
          },
          {
            subjectName: '123',
            price: 1234543
          },
          {
            subjectName: '123',
            price: 1234543
          },
        ]
      }
    },
    formatMoney (data, row, value) {
      // 格式化金额
      return AmountUtil.formatMoney(data, row, value)
    },
    handleClose () {
      // 关闭弹框
      this.$emit('close-model')
    }
  }
}
</script>

<style>
.invoice-sum-box .el-table__empty-block {
  min-height: 0;
}
</style>

<style lang="less" scoped>
@import '../../../style/base.less';
.case-detail {
  width: 100%;
  .table-main {
    // height: 150px;
  }
  .footer {
    text-align: center;
    margin: 15px 0 20px 0;
  }
}
</style>

```

```js

```



```js

```

