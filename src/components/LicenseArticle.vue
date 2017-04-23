<template>
    <div class="outer-container">
        <div class="publish_result">
            <transition name="fade">
                <p v-show="show_result" class="publish-active active-result">{{resMsg}}</p>
            </transition>
        </div>
        <div class="guide-wrapper">
            <div class="guide">
                <div class="line"></div> <!-- ngIf: $state.includes('app.home.publish.article') -->
                <div ng-if="$state.includes('app.home.publish.article')"
                     ng-class="{'step':true, 'active':$state.includes('app.home.publish.article.*')}"
                     class="ng-scope step active">
                    <div class="dot-wrapper">
                        <div class="dot"></div>
                    </div>
                    <div class="text">编辑文章</div>
                </div><!-- end ngIf: $state.includes('app.home.publish.article') -->
                <!-- ngIf: $state.includes('app.home.publish.image') -->
                <div
                    ng-class="{'step':true, 'active':$state.includes('app.home.publish.*.license') || $state.includes('app.home.publish.*.result')}"
                    class="step active">
                    <div class="dot-wrapper">
                        <div class="dot"></div>
                    </div>
                    <div class="text">选择协议</div>
                </div> <!-- ngIf: $state.includes('app.home.publish.article') -->
                <div ng-class="{'step':true, 'active':$state.is('app.home.publish.article.result')}"
                     ng-if="$state.includes('app.home.publish.article')" class="ng-scope step">
                    <div class="dot-wrapper">
                        <div class="dot"></div>
                    </div>
                    <div class="text">发布媒体</div>
                </div><!-- end ngIf: $state.includes('app.home.publish.article') -->
                <!-- ngIf: $state.includes('app.home.publish.image') -->
            </div>
        </div>
        <div class="page-content ng-scope" ui-view="">
            <publish-article article="article" license="license" clear-article-storage="$ctrl.clearArticleStorage()"
                             app-user="$ctrl.appUser" class="ng-scope ng-isolate-scope"><!-- uiView: -->
                <div ui-view="" class="ng-scope">
                    <publish-license license="license" last-state="app.home.publish.article.edit"
                                     publish="$ctrl.publish(license)" loading="loading" app-user="$ctrl.appUser"
                                     class="ng-scope ng-isolate-scope">
                        <div class="license-picker">
                            <div class="license-type">
                                <div class="description">1. 请选择转载许可协议:</div>
                                <div class="options">
                                    <div ng-class="{'option':true,'cc':true,'active':license.type=='cc'}"
                                         class="option cc active">
                                        <div class="icon" ng-click="license.type='cc'"><img
                                            src="data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz4KPCEtLSBHZW5lcmF0b3I6IEFkb2JlIElsbHVzdHJhdG9yIDEzLjAuMiwgU1ZHIEV4cG9ydCBQbHVnLUluIC4gU1ZHIFZlcnNpb246IDYuMDAgQnVpbGQgMTQ5NDgpICAtLT4KPCFET0NUWVBFIHN2ZyBQVUJMSUMgIi0vL1czQy8vRFREIFNWRyAxLjAvL0VOIiAiaHR0cDovL3d3dy53My5vcmcvVFIvMjAwMS9SRUMtU1ZHLTIwMDEwOTA0L0RURC9zdmcxMC5kdGQiPgo8c3ZnIHZlcnNpb249IjEuMCIgaWQ9IkxheWVyXzEiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyIgeG1sbnM6eGxpbms9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGxpbmsiIHg9IjBweCIgeT0iMHB4IgoJIHdpZHRoPSI2NHB4IiBoZWlnaHQ9IjY0cHgiIHZpZXdCb3g9IjUuNSAtMy41IDY0IDY0IiBlbmFibGUtYmFja2dyb3VuZD0ibmV3IDUuNSAtMy41IDY0IDY0IiB4bWw6c3BhY2U9InByZXNlcnZlIj4KPGc+Cgk8Y2lyY2xlIGZpbGw9IiNGRkZGRkYiIGN4PSIzNy43ODUiIGN5PSIyOC41MDEiIHI9IjI4LjgzNiIvPgoJPHBhdGggZD0iTTM3LjQ0MS0zLjVjOC45NTEsMCwxNi41NzIsMy4xMjUsMjIuODU3LDkuMzcyYzMuMDA4LDMuMDA5LDUuMjk1LDYuNDQ4LDYuODU3LDEwLjMxNAoJCWMxLjU2MSwzLjg2NywyLjM0NCw3Ljk3MSwyLjM0NCwxMi4zMTRjMCw0LjM4MS0wLjc3Myw4LjQ4Ni0yLjMxNCwxMi4zMTNjLTEuNTQzLDMuODI4LTMuODIsNy4yMS02LjgyOCwxMC4xNDMKCQljLTMuMTIzLDMuMDg1LTYuNjY2LDUuNDQ4LTEwLjYyOSw3LjA4NmMtMy45NjEsMS42MzgtOC4wNTcsMi40NTctMTIuMjg1LDIuNDU3cy04LjI3Ni0wLjgwOC0xMi4xNDMtMi40MjkKCQljLTMuODY2LTEuNjE4LTcuMzMzLTMuOTYxLTEwLjQtNy4wMjdjLTMuMDY3LTMuMDY2LTUuNC02LjUyNC03LTEwLjM3MlM1LjUsMzIuNzY3LDUuNSwyOC41YzAtNC4yMjksMC44MDktOC4yOTUsMi40MjgtMTIuMgoJCWMxLjYxOS0zLjkwNSwzLjk3Mi03LjQsNy4wNTctMTAuNDg2QzIxLjA4LTAuMzk0LDI4LjU2NS0zLjUsMzcuNDQxLTMuNXogTTM3LjU1NywyLjI3MmMtNy4zMTQsMC0xMy40NjcsMi41NTMtMTguNDU4LDcuNjU3CgkJYy0yLjUxNSwyLjU1My00LjQ0OCw1LjQxOS01LjgsOC42Yy0xLjM1NCwzLjE4MS0yLjAyOSw2LjUwNS0yLjAyOSw5Ljk3MmMwLDMuNDI5LDAuNjc1LDYuNzM0LDIuMDI5LDkuOTEzCgkJYzEuMzUzLDMuMTgzLDMuMjg1LDYuMDIxLDUuOCw4LjUxNmMyLjUxNCwyLjQ5Niw1LjM1MSw0LjM5OSw4LjUxNSw1LjcxNWMzLjE2MSwxLjMxNCw2LjQ3NiwxLjk3MSw5Ljk0MywxLjk3MQoJCWMzLjQyOCwwLDYuNzUtMC42NjUsOS45NzMtMS45OTljMy4yMTktMS4zMzUsNi4xMjEtMy4yNTcsOC43MTMtNS43NzFjNC45OS00Ljg3Niw3LjQ4NC0xMC45OSw3LjQ4NC0xOC4zNDQKCQljMC0zLjU0My0wLjY0OC02Ljg5NS0xLjk0My0xMC4wNTdjLTEuMjkzLTMuMTYyLTMuMTgtNS45OC01LjY1NC04LjQ1OEM1MC45ODQsNC44NDQsNDQuNzk1LDIuMjcyLDM3LjU1NywyLjI3MnogTTM3LjE1NiwyMy4xODcKCQlsLTQuMjg3LDIuMjI5Yy0wLjQ1OC0wLjk1MS0xLjAxOS0xLjYxOS0xLjY4NS0yYy0wLjY2Ny0wLjM4LTEuMjg2LTAuNTcxLTEuODU4LTAuNTcxYy0yLjg1NiwwLTQuMjg2LDEuODg1LTQuMjg2LDUuNjU3CgkJYzAsMS43MTQsMC4zNjIsMy4wODQsMS4wODUsNC4xMTNjMC43MjQsMS4wMjksMS43OTEsMS41NDQsMy4yMDEsMS41NDRjMS44NjcsMCwzLjE4MS0wLjkxNSwzLjk0NC0yLjc0M2wzLjk0MiwyCgkJYy0wLjgzOCwxLjU2My0yLDIuNzkxLTMuNDg2LDMuNjg2Yy0xLjQ4NCwwLjg5Ni0zLjEyMywxLjM0My00LjkxNCwxLjM0M2MtMi44NTcsMC01LjE2My0wLjg3NS02LjkxNS0yLjYyOQoJCWMtMS43NTItMS43NTItMi42MjgtNC4xOS0yLjYyOC03LjMxM2MwLTMuMDQ4LDAuODg2LTUuNDY2LDIuNjU3LTcuMjU3YzEuNzcxLTEuNzksNC4wMDktMi42ODYsNi43MTUtMi42ODYKCQlDMzIuNjA0LDE4LjU1OCwzNS40NDEsMjAuMTAxLDM3LjE1NiwyMy4xODd6IE01NS42MTMsMjMuMTg3bC00LjIyOSwyLjIyOWMtMC40NTctMC45NTEtMS4wMi0xLjYxOS0xLjY4Ni0yCgkJYy0wLjY2OC0wLjM4LTEuMzA3LTAuNTcxLTEuOTE0LTAuNTcxYy0yLjg1NywwLTQuMjg3LDEuODg1LTQuMjg3LDUuNjU3YzAsMS43MTQsMC4zNjMsMy4wODQsMS4wODYsNC4xMTMKCQljMC43MjMsMS4wMjksMS43ODksMS41NDQsMy4yMDEsMS41NDRjMS44NjUsMCwzLjE4LTAuOTE1LDMuOTQxLTIuNzQzbDQsMmMtMC44NzUsMS41NjMtMi4wNTcsMi43OTEtMy41NDEsMy42ODYKCQljLTEuNDg2LDAuODk2LTMuMTA1LDEuMzQzLTQuODU3LDEuMzQzYy0yLjg5NiwwLTUuMjA5LTAuODc1LTYuOTQxLTIuNjI5Yy0xLjczNi0xLjc1Mi0yLjYwMi00LjE5LTIuNjAyLTcuMzEzCgkJYzAtMy4wNDgsMC44ODUtNS40NjYsMi42NTgtNy4yNTdjMS43Ny0xLjc5LDQuMDA4LTIuNjg2LDYuNzEzLTIuNjg2QzUxLjExNywxOC41NTgsNTMuOTM4LDIwLjEwMSw1NS42MTMsMjMuMTg3eiIvPgo8L2c+Cjwvc3ZnPgo="
                                            width="64" height="64"></div>
                                        <label> <input type="radio" checked ng-model="license.type" ng-value="'cc'"
                                                       class="ng-valid ng-not-empty ng-dirty ng-valid-parse ng-touched"
                                                       name="112" value="cc"> 知识共享许可协议 </label>
                                        <div class="triangle"></div>
                                    </div>
                                    <div ng-class="{'option':true,'cm':true,'active':license.type=='cm'}"
                                         class="option cm">
                                        <div class="icon" ng-click="license.type='cm'"><img
                                            src="data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz4KPCEtLSBHZW5lcmF0b3I6IEFkb2JlIElsbHVzdHJhdG9yIDE3LjAuMCwgU1ZHIEV4cG9ydCBQbHVnLUluIC4gU1ZHIFZlcnNpb246IDYuMDAgQnVpbGQgMCkgIC0tPgo8IURPQ1RZUEUgc3ZnIFBVQkxJQyAiLS8vVzNDLy9EVEQgU1ZHIDEuMS8vRU4iICJodHRwOi8vd3d3LnczLm9yZy9HcmFwaGljcy9TVkcvMS4xL0RURC9zdmcxMS5kdGQiPgo8c3ZnIHZlcnNpb249IjEuMSIgaWQ9IuWbvuWxgl8xIiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHhtbG5zOnhsaW5rPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hsaW5rIiB4PSIwcHgiIHk9IjBweCIKCSB3aWR0aD0iNDhweCIgaGVpZ2h0PSI1N3B4IiB2aWV3Qm94PSIwIDAgNDggNTciIGVuYWJsZS1iYWNrZ3JvdW5kPSJuZXcgMCAwIDQ4IDU3IiB4bWw6c3BhY2U9InByZXNlcnZlIj4KPGc+Cgk8cGF0aCBmaWxsPSIjRkZGRkZGIiBkPSJNMi41NjIsNDIuODczVjEwLjMyMWg2LjM3M2M1LjUyOCwwLDExLjE5NC0yLjI5NywxNC45NzYtNS45MTNjMy43ODIsMy42MTYsOS40NDgsNS45MTMsMTQuOTc2LDUuOTEzaDYuMzcyCgkJdjMyLjU1MkwyMy45MTEsNTMuOTdMMi41NjIsNDIuODczeiIvPgoJPHBhdGggZD0iTTIzLjkxMSw3LjcxOGM0LjExMiwzLjE5Myw5LjUzMiw1LjEwMywxNC45NzYsNS4xMDNoMy44NzJ2MjguNTM0bC0xOC44NDgsOS43OTdMNS4wNjMsNDEuMzU1VjEyLjgyMWgzLjg3MgoJCUMxNC4zNzksMTIuODIxLDE5Ljc5OSwxMC45MTEsMjMuOTExLDcuNzE4IE0yMy45MTEsMC42MjVjLTMuMTIyLDQuMzUyLTkuMjExLDcuMTk2LTE0Ljk3Niw3LjE5NkgwLjA2M3YzNi41NzFsMjMuODQ4LDEyLjM5NgoJCWwyMy44NDgtMTIuMzk2VjcuODIxaC04Ljg3MkMzMy4xMjIsNy44MjEsMjcuMDMzLDQuOTc2LDIzLjkxMSwwLjYyNUwyMy45MTEsMC42MjV6Ii8+CjwvZz4KPHJlY3QgeD0iMTIuNjI1IiB5PSIyNS4xOSIgZmlsbD0iIzA0MDAwMCIgd2lkdGg9IjIyLjU3MiIgaGVpZ2h0PSI1Ii8+CjxyZWN0IHg9IjEzLjY3NiIgeT0iMTguNzM1IiB0cmFuc2Zvcm09Im1hdHJpeCgwLjUxOTYgMC44NTQ0IC0wLjg1NDQgMC41MTk2IDI3LjYxMjMgLTcuMTY2NCkiIGZpbGw9IiMwNDAwMDAiIHdpZHRoPSIxMy4wMDYiIGhlaWdodD0iNC40NzQiLz4KPHJlY3QgeD0iMjEuMTk4IiB5PSIxOC43MzUiIHRyYW5zZm9ybT0ibWF0cml4KC0wLjUxOTYgMC44NTQ0IC0wLjg1NDQgLTAuNTE5NiA2MC4wMTMgOC4yMDA5KSIgZmlsbD0iIzA0MDAwMCIgd2lkdGg9IjEzLjAwNiIgaGVpZ2h0PSI0LjQ3NCIvPgo8cmVjdCB4PSIyMS40MTEiIHk9IjI1Ljg5MyIgZmlsbD0iIzA0MDAwMCIgd2lkdGg9IjUiIGhlaWdodD0iMTguNjA3Ii8+CjxyZWN0IHg9IjEyLjYyNSIgeT0iMzMuNTA2IiBmaWxsPSIjMDQwMDAwIiB3aWR0aD0iMjIuNTcyIiBoZWlnaHQ9IjUiLz4KPC9zdmc+Cg=="
                                            width="64" height="64"></div>
                                        <label> <input type="radio" disabled ng-model="license.type" ng-value="'cm'"
                                                       class="ng-valid ng-not-empty ng-dirty ng-touched" name="113"
                                                       value="cm">
                                            商业许可协议 </label>
                                        <div class="triangle"></div>
                                    </div>
                                </div>
                            </div>
                            <div class="license-detail"> <!-- ngIf: license.type=='cc' -->
                                <div class="cc-adaptation license-item ng-scope" ng-if="license.type=='cc'">
                                    <div class="description">2. 是否允许您的作品被改编:</div>
                                    <div class="question n3"><label> <input type="radio"
                                                                            ng-model="license.cc.adaptation"
                                                                            ng-value="'y'"
                                                                            class="ng-pristine ng-untouched ng-valid ng-not-empty"
                                                                            name="161" value="y" v-model="adapt"> 是
                                    </label> <label>
                                        <input
                                            type="radio" ng-model="license.cc.adaptation" ng-value="'n'"
                                            class="ng-pristine ng-untouched ng-valid ng-not-empty" name="162" value="n"
                                            v-model="adapt">
                                        否 </label>
                                        <label> <input type="radio" ng-model="license.cc.adaptation"
                                                       ng-value="'sa'"
                                                       class="ng-pristine ng-untouched ng-valid ng-not-empty" name="163"
                                                       value="sa" v-model="adapt"> 是，只要他人以相同方式共享 </label></div>
                                </div><!-- end ngIf: license.type=='cc' --> <!-- ngIf: license.type=='cc' -->
                                <div class="cc-commercial license-item ng-scope" ng-if="license.type=='cc'">
                                    <div class="description">3. 是否允许商业使用:</div>
                                    <div class="question n2"><label> <input type="radio"
                                                                            ng-model="license.cc.commercial"
                                                                            ng-value="'y'"
                                                                            class="ng-pristine ng-untouched ng-valid ng-not-empty"
                                                                            name="165" value="y" v-model="commercial"> 是
                                    </label> <label>
                                        <input
                                            type="radio" ng-model="license.cc.commercial" ng-value="'n'"
                                            class="ng-pristine ng-untouched ng-valid ng-not-empty" name="166" value="n"
                                            v-model="commercial">
                                        否 </label>
                                    </div>
                                </div><!-- end ngIf: license.type=='cc' --> <!-- ngIf: license.type=='cm' -->
                                <!-- ngIf: license.type=='cm' --> </div>
                            <div class="license-preview" ng-show="license.type=='cc'">
                                <div class="name">您选择的转载许可协议：</div>
                                <license license="licensePreview" class="ng-isolate-scope">
                                    <div class="license"> <!-- ngIf: !$ctrl.licenseConfirm -->
                                        <license-icons license="$ctrl.license" ng-if="!$ctrl.licenseConfirm"
                                                       class="ng-scope ng-isolate-scope">
                                            <div class="icons"> <!-- ngIf: $ctrl.license.type=='cc' -->
                                                <div class="cc ng-scope" ng-if="$ctrl.license.type=='cc'"><a
                                                    href="javascript:void(0)" title="知识共享（CC）4.0协议"> <img
                                                    src="data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz4KPCEtLSBHZW5lcmF0b3I6IEFkb2JlIElsbHVzdHJhdG9yIDEzLjAuMiwgU1ZHIEV4cG9ydCBQbHVnLUluIC4gU1ZHIFZlcnNpb246IDYuMDAgQnVpbGQgMTQ5NDgpICAtLT4KPCFET0NUWVBFIHN2ZyBQVUJMSUMgIi0vL1czQy8vRFREIFNWRyAxLjAvL0VOIiAiaHR0cDovL3d3dy53My5vcmcvVFIvMjAwMS9SRUMtU1ZHLTIwMDEwOTA0L0RURC9zdmcxMC5kdGQiPgo8c3ZnIHZlcnNpb249IjEuMCIgaWQ9IkxheWVyXzEiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyIgeG1sbnM6eGxpbms9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGxpbmsiIHg9IjBweCIgeT0iMHB4IgoJIHdpZHRoPSI2NHB4IiBoZWlnaHQ9IjY0cHgiIHZpZXdCb3g9IjUuNSAtMy41IDY0IDY0IiBlbmFibGUtYmFja2dyb3VuZD0ibmV3IDUuNSAtMy41IDY0IDY0IiB4bWw6c3BhY2U9InByZXNlcnZlIj4KPGc+Cgk8Y2lyY2xlIGZpbGw9IiNGRkZGRkYiIGN4PSIzNy43ODUiIGN5PSIyOC41MDEiIHI9IjI4LjgzNiIvPgoJPHBhdGggZD0iTTM3LjQ0MS0zLjVjOC45NTEsMCwxNi41NzIsMy4xMjUsMjIuODU3LDkuMzcyYzMuMDA4LDMuMDA5LDUuMjk1LDYuNDQ4LDYuODU3LDEwLjMxNAoJCWMxLjU2MSwzLjg2NywyLjM0NCw3Ljk3MSwyLjM0NCwxMi4zMTRjMCw0LjM4MS0wLjc3Myw4LjQ4Ni0yLjMxNCwxMi4zMTNjLTEuNTQzLDMuODI4LTMuODIsNy4yMS02LjgyOCwxMC4xNDMKCQljLTMuMTIzLDMuMDg1LTYuNjY2LDUuNDQ4LTEwLjYyOSw3LjA4NmMtMy45NjEsMS42MzgtOC4wNTcsMi40NTctMTIuMjg1LDIuNDU3cy04LjI3Ni0wLjgwOC0xMi4xNDMtMi40MjkKCQljLTMuODY2LTEuNjE4LTcuMzMzLTMuOTYxLTEwLjQtNy4wMjdjLTMuMDY3LTMuMDY2LTUuNC02LjUyNC03LTEwLjM3MlM1LjUsMzIuNzY3LDUuNSwyOC41YzAtNC4yMjksMC44MDktOC4yOTUsMi40MjgtMTIuMgoJCWMxLjYxOS0zLjkwNSwzLjk3Mi03LjQsNy4wNTctMTAuNDg2QzIxLjA4LTAuMzk0LDI4LjU2NS0zLjUsMzcuNDQxLTMuNXogTTM3LjU1NywyLjI3MmMtNy4zMTQsMC0xMy40NjcsMi41NTMtMTguNDU4LDcuNjU3CgkJYy0yLjUxNSwyLjU1My00LjQ0OCw1LjQxOS01LjgsOC42Yy0xLjM1NCwzLjE4MS0yLjAyOSw2LjUwNS0yLjAyOSw5Ljk3MmMwLDMuNDI5LDAuNjc1LDYuNzM0LDIuMDI5LDkuOTEzCgkJYzEuMzUzLDMuMTgzLDMuMjg1LDYuMDIxLDUuOCw4LjUxNmMyLjUxNCwyLjQ5Niw1LjM1MSw0LjM5OSw4LjUxNSw1LjcxNWMzLjE2MSwxLjMxNCw2LjQ3NiwxLjk3MSw5Ljk0MywxLjk3MQoJCWMzLjQyOCwwLDYuNzUtMC42NjUsOS45NzMtMS45OTljMy4yMTktMS4zMzUsNi4xMjEtMy4yNTcsOC43MTMtNS43NzFjNC45OS00Ljg3Niw3LjQ4NC0xMC45OSw3LjQ4NC0xOC4zNDQKCQljMC0zLjU0My0wLjY0OC02Ljg5NS0xLjk0My0xMC4wNTdjLTEuMjkzLTMuMTYyLTMuMTgtNS45OC01LjY1NC04LjQ1OEM1MC45ODQsNC44NDQsNDQuNzk1LDIuMjcyLDM3LjU1NywyLjI3MnogTTM3LjE1NiwyMy4xODcKCQlsLTQuMjg3LDIuMjI5Yy0wLjQ1OC0wLjk1MS0xLjAxOS0xLjYxOS0xLjY4NS0yYy0wLjY2Ny0wLjM4LTEuMjg2LTAuNTcxLTEuODU4LTAuNTcxYy0yLjg1NiwwLTQuMjg2LDEuODg1LTQuMjg2LDUuNjU3CgkJYzAsMS43MTQsMC4zNjIsMy4wODQsMS4wODUsNC4xMTNjMC43MjQsMS4wMjksMS43OTEsMS41NDQsMy4yMDEsMS41NDRjMS44NjcsMCwzLjE4MS0wLjkxNSwzLjk0NC0yLjc0M2wzLjk0MiwyCgkJYy0wLjgzOCwxLjU2My0yLDIuNzkxLTMuNDg2LDMuNjg2Yy0xLjQ4NCwwLjg5Ni0zLjEyMywxLjM0My00LjkxNCwxLjM0M2MtMi44NTcsMC01LjE2My0wLjg3NS02LjkxNS0yLjYyOQoJCWMtMS43NTItMS43NTItMi42MjgtNC4xOS0yLjYyOC03LjMxM2MwLTMuMDQ4LDAuODg2LTUuNDY2LDIuNjU3LTcuMjU3YzEuNzcxLTEuNzksNC4wMDktMi42ODYsNi43MTUtMi42ODYKCQlDMzIuNjA0LDE4LjU1OCwzNS40NDEsMjAuMTAxLDM3LjE1NiwyMy4xODd6IE01NS42MTMsMjMuMTg3bC00LjIyOSwyLjIyOWMtMC40NTctMC45NTEtMS4wMi0xLjYxOS0xLjY4Ni0yCgkJYy0wLjY2OC0wLjM4LTEuMzA3LTAuNTcxLTEuOTE0LTAuNTcxYy0yLjg1NywwLTQuMjg3LDEuODg1LTQuMjg3LDUuNjU3YzAsMS43MTQsMC4zNjMsMy4wODQsMS4wODYsNC4xMTMKCQljMC43MjMsMS4wMjksMS43ODksMS41NDQsMy4yMDEsMS41NDRjMS44NjUsMCwzLjE4LTAuOTE1LDMuOTQxLTIuNzQzbDQsMmMtMC44NzUsMS41NjMtMi4wNTcsMi43OTEtMy41NDEsMy42ODYKCQljLTEuNDg2LDAuODk2LTMuMTA1LDEuMzQzLTQuODU3LDEuMzQzYy0yLjg5NiwwLTUuMjA5LTAuODc1LTYuOTQxLTIuNjI5Yy0xLjczNi0xLjc1Mi0yLjYwMi00LjE5LTIuNjAyLTcuMzEzCgkJYzAtMy4wNDgsMC44ODUtNS40NjYsMi42NTgtNy4yNTdjMS43Ny0xLjc5LDQuMDA4LTIuNjg2LDYuNzEzLTIuNjg2QzUxLjExNywxOC41NTgsNTMuOTM4LDIwLjEwMSw1NS42MTMsMjMuMTg3eiIvPgo8L2c+Cjwvc3ZnPgo=">
                                                </a> <a href="javascript:void(0)" title="必须按照许可人指定的方式对作品进行署名"> <img
                                                    src="data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz4KPCEtLSBHZW5lcmF0b3I6IEFkb2JlIElsbHVzdHJhdG9yIDEzLjAuMiwgU1ZHIEV4cG9ydCBQbHVnLUluIC4gU1ZHIFZlcnNpb246IDYuMDAgQnVpbGQgMTQ5NDgpICAtLT4KPCFET0NUWVBFIHN2ZyBQVUJMSUMgIi0vL1czQy8vRFREIFNWRyAxLjAvL0VOIiAiaHR0cDovL3d3dy53My5vcmcvVFIvMjAwMS9SRUMtU1ZHLTIwMDEwOTA0L0RURC9zdmcxMC5kdGQiPgo8c3ZnIHZlcnNpb249IjEuMCIgaWQ9IkxheWVyXzEiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyIgeG1sbnM6eGxpbms9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGxpbmsiIHg9IjBweCIgeT0iMHB4IgoJIHdpZHRoPSI2NHB4IiBoZWlnaHQ9IjY0cHgiIHZpZXdCb3g9IjUuNSAtMy41IDY0IDY0IiBlbmFibGUtYmFja2dyb3VuZD0ibmV3IDUuNSAtMy41IDY0IDY0IiB4bWw6c3BhY2U9InByZXNlcnZlIj4KPGc+Cgk8Y2lyY2xlIGZpbGw9IiNGRkZGRkYiIGN4PSIzNy42MzciIGN5PSIyOC44MDYiIHI9IjI4LjI3NiIvPgoJPGc+CgkJPHBhdGggZD0iTTM3LjQ0My0zLjVjOC45ODgsMCwxNi41NywzLjA4NSwyMi43NDIsOS4yNTdDNjYuMzkzLDExLjk2Nyw2OS41LDE5LjU0OCw2OS41LDI4LjVjMCw4Ljk5MS0zLjA0OSwxNi40NzYtOS4xNDUsMjIuNDU2CgkJCUM1My44NzksNTcuMzE5LDQ2LjI0Miw2MC41LDM3LjQ0Myw2MC41Yy04LjY0OSwwLTE2LjE1My0zLjE0NC0yMi41MTQtOS40M0M4LjY0NCw0NC43ODQsNS41LDM3LjI2Miw1LjUsMjguNQoJCQljMC04Ljc2MSwzLjE0NC0xNi4zNDIsOS40MjktMjIuNzQyQzIxLjEwMS0wLjQxNSwyOC42MDQtMy41LDM3LjQ0My0zLjV6IE0zNy41NTcsMi4yNzJjLTcuMjc2LDAtMTMuNDI4LDIuNTUzLTE4LjQ1Nyw3LjY1NwoJCQljLTUuMjIsNS4zMzQtNy44MjksMTEuNTI1LTcuODI5LDE4LjU3MmMwLDcuMDg2LDIuNTksMTMuMjIsNy43NywxOC4zOThjNS4xODEsNS4xODIsMTEuMzUyLDcuNzcxLDE4LjUxNCw3Ljc3MQoJCQljNy4xMjMsMCwxMy4zMzQtMi42MDcsMTguNjI5LTcuODI4YzUuMDI5LTQuODM4LDcuNTQzLTEwLjk1Miw3LjU0My0xOC4zNDNjMC03LjI3Ni0yLjU1My0xMy40NjUtNy42NTYtMTguNTcxCgkJCUM1MC45NjcsNC44MjQsNDQuNzk1LDIuMjcyLDM3LjU1NywyLjI3MnogTTQ2LjEyOSwyMC41NTd2MTMuMDg1aC0zLjY1NnYxNS41NDJoLTkuOTQ0VjMzLjY0M2gtMy42NTZWMjAuNTU3CgkJCWMwLTAuNTcyLDAuMi0xLjA1NywwLjU5OS0xLjQ1N2MwLjQwMS0wLjM5OSwwLjg4Ny0wLjYsMS40NTctMC42aDEzLjE0NGMwLjUzMywwLDEuMDEsMC4yLDEuNDI4LDAuNgoJCQlDNDUuOTE4LDE5LjUsNDYuMTI5LDE5Ljk4Niw0Ni4xMjksMjAuNTU3eiBNMzMuMDQyLDEyLjMyOWMwLTMuMDA4LDEuNDg1LTQuNTE0LDQuNDU4LTQuNTE0czQuNDU3LDEuNTA0LDQuNDU3LDQuNTE0CgkJCWMwLDIuOTcxLTEuNDg2LDQuNDU3LTQuNDU3LDQuNDU3UzMzLjA0MiwxNS4zLDMzLjA0MiwxMi4zMjl6Ii8+Cgk8L2c+CjwvZz4KPC9zdmc+Cg==">
                                                </a> <a href="javascript:void(0)" title="仅被授权出于非商业目的而复制、发行、展览和表演作品"
                                                        ng-show="$ctrl.license.content.commercial=='n'" class="ng-hide">
                                                    <img
                                                        src="data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz4KPCEtLSBHZW5lcmF0b3I6IEFkb2JlIElsbHVzdHJhdG9yIDEzLjAuMiwgU1ZHIEV4cG9ydCBQbHVnLUluIC4gU1ZHIFZlcnNpb246IDYuMDAgQnVpbGQgMTQ5NDgpICAtLT4KPCFET0NUWVBFIHN2ZyBQVUJMSUMgIi0vL1czQy8vRFREIFNWRyAxLjAvL0VOIiAiaHR0cDovL3d3dy53My5vcmcvVFIvMjAwMS9SRUMtU1ZHLTIwMDEwOTA0L0RURC9zdmcxMC5kdGQiPgo8c3ZnIHZlcnNpb249IjEuMCIgaWQ9IkxheWVyXzEiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyIgeG1sbnM6eGxpbms9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGxpbmsiIHg9IjBweCIgeT0iMHB4IgoJIHdpZHRoPSI2NHB4IiBoZWlnaHQ9IjY0cHgiIHZpZXdCb3g9IjUuNSAtMy41IDY0IDY0IiBlbmFibGUtYmFja2dyb3VuZD0ibmV3IDUuNSAtMy41IDY0IDY0IiB4bWw6c3BhY2U9InByZXNlcnZlIj4KPGc+Cgk8Y2lyY2xlIGZpbGw9IiNGRkZGRkYiIGN4PSIzNy40NyIgY3k9IjI4LjczNiIgcj0iMjkuNDcxIi8+Cgk8Zz4KCQk8cGF0aCBkPSJNMzcuNDQyLTMuNWM4Ljk5LDAsMTYuNTcxLDMuMDg1LDIyLjc0Myw5LjI1NkM2Ni4zOTMsMTEuOTI4LDY5LjUsMTkuNTA5LDY5LjUsMjguNWMwLDguOTkyLTMuMDQ4LDE2LjQ3Ni05LjE0NSwyMi40NTgKCQkJQzUzLjg4LDU3LjMyLDQ2LjI0MSw2MC41LDM3LjQ0Miw2MC41Yy04LjY4NiwwLTE2LjE5LTMuMTYyLTIyLjUxMy05LjQ4NUM4LjY0NCw0NC43MjgsNS41LDM3LjIyNSw1LjUsMjguNQoJCQljMC04Ljc2MiwzLjE0NC0xNi4zNDMsOS40MjktMjIuNzQzQzIxLjEtMC40MTQsMjguNjA0LTMuNSwzNy40NDItMy41eiBNMTIuNywxOS44NzJjLTAuOTUyLDIuNjI4LTEuNDI5LDUuNTA1LTEuNDI5LDguNjI5CgkJCWMwLDcuMDg2LDIuNTksMTMuMjIsNy43NywxOC40YzUuMjE5LDUuMTQ0LDExLjM5MSw3LjcxNSwxOC41MTQsNy43MTVjNy4yMDEsMCwxMy40MDktMi42MDgsMTguNjMtNy44MjkKCQkJYzEuODY3LTEuNzksMy4zMzItMy42NTcsNC4zOTgtNS42MDJsLTEyLjA1Ni01LjM3MWMtMC40MjEsMi4wMi0xLjQzOSwzLjY2Ny0zLjA1Nyw0Ljk0MmMtMS42MjIsMS4yNzYtMy41MzUsMi4wMTEtNS43NDQsMi4yCgkJCXY0LjkxNWgtMy43MTR2LTQuOTE1Yy0zLjU0My0wLjAzNi02Ljc4Mi0xLjMxMi05LjcxNC0zLjgyN2w0LjQtNC40NTdjMi4wOTQsMS45NDIsNC40NzYsMi45MTMsNy4xNDMsMi45MTMKCQkJYzEuMTA0LDAsMi4wNDgtMC4yNDYsMi44My0wLjc0M2MwLjc4LTAuNDk0LDEuMTcyLTEuMzEyLDEuMTcyLTIuNDU3YzAtMC44MDEtMC4yODctMS40NDgtMC44NTgtMS45NDNsLTMuMDg1LTEuMzE1bC0zLjc3MS0xLjcxNQoJCQlsLTUuMDg2LTIuMjI5TDEyLjcsMTkuODcyeiBNMzcuNTU3LDIuMjE0Yy03LjI3NiwwLTEzLjQyOCwyLjU3MS0xOC40NTcsNy43MTRjLTEuMjU4LDEuMjU4LTIuNDM5LDIuNjg2LTMuNTQzLDQuMjg3TDI3Ljc4NiwxOS43CgkJCWMwLjUzMy0xLjY3NiwxLjU0Mi0zLjAxOSwzLjAyOS00LjAyOGMxLjQ4NC0xLjAwOSwzLjIxOC0xLjU3MSw1LjItMS42ODZWOS4wNzFoMy43MTV2NC45MTVjMi45MzQsMC4xNTMsNS42LDEuMTQzLDgsMi45NzEKCQkJbC00LjE3Miw0LjI4NmMtMS43OTMtMS4yNTctMy42MTktMS44ODUtNS40ODYtMS44ODVjLTAuOTkxLDAtMS44NzYsMC4xOTEtMi42NTYsMC41NzFjLTAuNzgxLDAuMzgxLTEuMTcyLDEuMDI5LTEuMTcyLDEuOTQzCgkJCWMwLDAuMjY3LDAuMDk1LDAuNTMzLDAuMjg1LDAuOGw0LjA1NywxLjgzbDIuOCwxLjI1N2w1LjE0NCwyLjI4NWwxNi4zOTcsNy4zMTRjMC41MzUtMi4yNDgsMC44MDEtNC41MzMsMC44MDEtNi44NTcKCQkJYzAtNy4zNTMtMi41NTItMTMuNTQzLTcuNjU2LTE4LjU3M0M1MS4wMDUsNC43ODUsNDQuODMxLDIuMjE0LDM3LjU1NywyLjIxNHoiLz4KCTwvZz4KPC9nPgo8L3N2Zz4K">
                                                </a> <a href="javascript:void(0)" title="不允许基于该作品创作演绎作品"
                                                        ng-show="$ctrl.license.content.adaptation=='n'" class="ng-hide">
                                                    <img
                                                        src="data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz4KPCEtLSBHZW5lcmF0b3I6IEFkb2JlIElsbHVzdHJhdG9yIDEzLjAuMiwgU1ZHIEV4cG9ydCBQbHVnLUluIC4gU1ZHIFZlcnNpb246IDYuMDAgQnVpbGQgMTQ5NDgpICAtLT4KPCFET0NUWVBFIHN2ZyBQVUJMSUMgIi0vL1czQy8vRFREIFNWRyAxLjAvL0VOIiAiaHR0cDovL3d3dy53My5vcmcvVFIvMjAwMS9SRUMtU1ZHLTIwMDEwOTA0L0RURC9zdmcxMC5kdGQiPgo8c3ZnIHZlcnNpb249IjEuMCIgaWQ9IkxheWVyXzEiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyIgeG1sbnM6eGxpbms9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGxpbmsiIHg9IjBweCIgeT0iMHB4IgoJIHdpZHRoPSI2NHB4IiBoZWlnaHQ9IjY0cHgiIHZpZXdCb3g9IjUgLTMgNjQgNjQiIGVuYWJsZS1iYWNrZ3JvdW5kPSJuZXcgMTEgLTcgNjQgNjQiIHhtbDpzcGFjZT0icHJlc2VydmUiPgo8Zz4KCTxjaXJjbGUgZmlsbD0iI0ZGRkZGRiIgY3g9IjM3LjU2NCIgY3k9IjI4LjI4OCIgcj0iMjkuMDEzIi8+Cgk8Zz4KCQk8cGF0aCBkPSJNMzcuNDQzLTMuNWM4Ljk1MSwwLDE2LjUzMSwzLjEwNSwyMi43NDIsOS4zMTVDNjYuMzkzLDExLjk4Nyw2OS41LDE5LjU0OCw2OS41LDI4LjVjMCw4Ljk1NC0zLjA0OSwxNi40NTctOS4xNDUsMjIuNTE0CgkJCUM1My45MTgsNTcuMzM4LDQ2LjI3OSw2MC41LDM3LjQ0Myw2MC41Yy04LjY0OSwwLTE2LjE1My0zLjE0My0yMi41MTQtOS40M0M4LjY0NCw0NC43ODYsNS41LDM3LjI2NCw1LjUsMjguNTAxCgkJCWMwLTguNzIzLDMuMTQ0LTE2LjI4NSw5LjQyOS0yMi42ODVDMjEuMTM4LTAuMzk1LDI4LjY0My0zLjUsMzcuNDQzLTMuNXogTTM3LjU1NywyLjI3MmMtNy4yNzYsMC0xMy40MjgsMi41NzItMTguNDU3LDcuNzE1CgkJCWMtNS4yMiw1LjI5Ni03LjgyOSwxMS40NjctNy44MjksMTguNTEzYzAsNy4xMjUsMi41OSwxMy4yNTcsNy43NywxOC40YzUuMTgxLDUuMTgyLDExLjM1Miw3Ljc3MSwxOC41MTQsNy43NzEKCQkJYzcuMTIzLDAsMTMuMzM0LTIuNjA4LDE4LjYyOS03LjgyOGM1LjAyOS00Ljg3Niw3LjU0My0xMC45ODksNy41NDMtMTguMzQzYzAtNy4zMTMtMi41NTMtMTMuNDg1LTcuNjU2LTE4LjUxMwoJCQlDNTEuMDA0LDQuODQyLDQ0LjgzMiwyLjI3MiwzNy41NTcsMi4yNzJ6IE00OS42MTUsMjAuOTU2djUuNDg2SDI2LjM1OHYtNS40ODZINDkuNjE1eiBNNDkuNjE1LDMxLjI0M3Y1LjQ4M0gyNi4zNTh2LTUuNDgzSDQ5LjYxNQoJCQl6Ii8+Cgk8L2c+CjwvZz4KPC9zdmc+Cg==">
                                                </a> <a href="javascript:void(0)"
                                                        title="您可以复制、发行、展览和表演作品，也必须允许他人基于该作品创作演绎作品"
                                                        ng-show="$ctrl.license.content.adaptation=='sa'"> <img
                                                    src="data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz4KPCEtLSBHZW5lcmF0b3I6IEFkb2JlIElsbHVzdHJhdG9yIDEzLjAuMiwgU1ZHIEV4cG9ydCBQbHVnLUluIC4gU1ZHIFZlcnNpb246IDYuMDAgQnVpbGQgMTQ5NDgpICAtLT4KPCFET0NUWVBFIHN2ZyBQVUJMSUMgIi0vL1czQy8vRFREIFNWRyAxLjAvL0VOIiAiaHR0cDovL3d3dy53My5vcmcvVFIvMjAwMS9SRUMtU1ZHLTIwMDEwOTA0L0RURC9zdmcxMC5kdGQiPgo8c3ZnIHZlcnNpb249IjEuMCIgaWQ9IkxheWVyXzEiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyIgeG1sbnM6eGxpbms9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGxpbmsiIHg9IjBweCIgeT0iMHB4IgoJIHdpZHRoPSI2NHB4IiBoZWlnaHQ9IjY0cHgiIHZpZXdCb3g9IjUuNSAtMy41IDY0IDY0IiBlbmFibGUtYmFja2dyb3VuZD0ibmV3IDUuNSAtMy41IDY0IDY0IiB4bWw6c3BhY2U9InByZXNlcnZlIj4KPGc+Cgk8Y2lyY2xlIGZpbGw9IiNGRkZGRkYiIGN4PSIzNi45NDQiIGN5PSIyOC42MzEiIHI9IjI5LjEwNSIvPgoJPGc+CgkJPHBhdGggZD0iTTM3LjQ0My0zLjVjOC45NTEsMCwxNi41MzEsMy4xMDUsMjIuNzQyLDkuMzE1QzY2LjM5MywxMS45ODcsNjkuNSwxOS41NDgsNjkuNSwyOC41YzAsOC45NTQtMy4wNDksMTYuNDU3LTkuMTQ1LDIyLjUxNAoJCQlDNTMuOTE4LDU3LjMzOCw0Ni4yNzksNjAuNSwzNy40NDMsNjAuNWMtOC42NDksMC0xNi4xNTMtMy4xNDMtMjIuNTE0LTkuNDI5QzguNjQ0LDQ0Ljc4Niw1LjUsMzcuMjY0LDUuNSwyOC41MDEKCQkJYzAtOC43MjMsMy4xNDQtMTYuMjg1LDkuNDI5LTIyLjY4NUMyMS4xMzgtMC4zOTUsMjguNjQzLTMuNSwzNy40NDMtMy41eiBNMzcuNTU3LDIuMjcyYy03LjI3NiwwLTEzLjQyOCwyLjU3Mi0xOC40NTcsNy43MTUKCQkJYy01LjIyLDUuMjk2LTcuODI5LDExLjQ2Ny03LjgyOSwxOC41MTNjMCw3LjEyNSwyLjU5LDEzLjI1Nyw3Ljc3LDE4LjRjNS4xODEsNS4xODIsMTEuMzUyLDcuNzcxLDE4LjUxNCw3Ljc3MQoJCQljNy4xMjMsMCwxMy4zMzQtMi42MDksMTguNjI5LTcuODI4YzUuMDI5LTQuODc2LDcuNTQzLTEwLjk5LDcuNTQzLTE4LjM0M2MwLTcuMzEzLTIuNTUzLTEzLjQ4NS03LjY1Ni0xOC41MTMKCQkJQzUxLjAwNCw0Ljg0Miw0NC44MzIsMi4yNzIsMzcuNTU3LDIuMjcyeiBNMjMuMjcxLDIzLjk4NWMwLjYwOS0zLjkyNCwyLjE4OS02Ljk2Miw0Ljc0Mi05LjExNAoJCQljMi41NTItMi4xNTIsNS42NTYtMy4yMjgsOS4zMTQtMy4yMjhjNS4wMjcsMCw5LjAyOSwxLjYyLDEyLDQuODU2YzIuOTcxLDMuMjM4LDQuNDU3LDcuMzkxLDQuNDU3LDEyLjQ1NwoJCQljMCw0LjkxNS0xLjU0Myw5LTQuNjI3LDEyLjI1NmMtMy4wODgsMy4yNTYtNy4wODYsNC44ODYtMTIuMDAyLDQuODg2Yy0zLjYxOSwwLTYuNzQzLTEuMDg1LTkuMzcxLTMuMjU3CgkJCWMtMi42MjktMi4xNzItNC4yMDktNS4yNTctNC43NDMtOS4yNTdIMzEuMWMwLjE5LDMuODg2LDIuNTMzLDUuODI5LDcuMDI5LDUuODI5YzIuMjQ2LDAsNC4wNTctMC45NzIsNS40MjgtMi45MTQKCQkJYzEuMzczLTEuOTQyLDIuMDU5LTQuNTM0LDIuMDU5LTcuNzcxYzAtMy4zOTEtMC42MjktNS45NzEtMS44ODUtNy43NDNjLTEuMjU4LTEuNzcxLTMuMDY2LTIuNjU3LTUuNDMtMi42NTcKCQkJYy00LjI2OCwwLTYuNjY3LDEuODg1LTcuMiw1LjY1NmgyLjM0M2wtNi4zNDIsNi4zNDNsLTYuMzQzLTYuMzQzTDIzLjI3MSwyMy45ODVMMjMuMjcxLDIzLjk4NXoiLz4KCTwvZz4KPC9nPgo8L3N2Zz4K">
                                                </a></div><!-- end ngIf: $ctrl.license.type=='cc' -->
                                                <!-- ngIf: $ctrl.license.type=='cm' --> </div>
                                        </license-icons><!-- end ngIf: !$ctrl.licenseConfirm -->
                                        <div ng-class="{'text': true, 'no-title': $ctrl.licenseConfirm}" class="text">
                                            <div class="cc" ng-show="$ctrl.license.type=='cc'">
                                                <!-- ngIf: !$ctrl.licenseConfirm --><span ng-if="!$ctrl.licenseConfirm"
                                                                                          class="ng-scope">本作品采用
                                                <!-- ngIf: $ctrl.license.content.commercial=='n' && $ctrl.license.content.adaptation=='n' -->
                                                <!-- ngIf: $ctrl.license.content.commercial=='n' && $ctrl.license.content.adaptation=='y' -->
                                                <!-- ngIf: $ctrl.license.content.commercial=='n' && $ctrl.license.content.adaptation=='sa' -->
                                                <!-- ngIf: $ctrl.license.content.commercial=='y' && $ctrl.license.content.adaptation=='n' -->
                                                <!-- ngIf: $ctrl.license.content.commercial=='y' && $ctrl.license.content.adaptation=='y' -->
                                                <!-- ngIf: $ctrl.license.content.commercial=='y' && $ctrl.license.content.adaptation=='sa' --><a
                                                    class="license-link ng-scope"
                                                    href="http://creativecommons.org/licenses/by-sa/4.0"
                                                    ng-if="$ctrl.license.content.commercial=='y' &amp;&amp; $ctrl.license.content.adaptation=='sa'"
                                                    target="_blank">知识共享署名-相同方式共享 4.0 国际许可协议</a>
                                                <!-- end ngIf: $ctrl.license.content.commercial=='y' && $ctrl.license.content.adaptation=='sa' --> 进行许可。</span>
                                                <!-- end ngIf: !$ctrl.licenseConfirm -->
                                                <ul class="details"> <!-- ngIf: $ctrl.licenseConfirm -->
                                                    <li>允许复制、发行、展览和表演作品，但必须按照指定的方式对作品进行署名</li>
                                                    <!-- ngIf: $ctrl.license.content.commercial=='n' -->
                                                    <!-- ngIf: $ctrl.license.content.commercial=='y' -->
                                                    <li ng-if="$ctrl.license.content.commercial=='y'" class="ng-scope">
                                                        允许复制、发行、展览和表演作品，包括出于商业目的进行上述活动
                                                    </li><!-- end ngIf: $ctrl.license.content.commercial=='y' -->
                                                    <!-- ngIf: $ctrl.license.content.adaptation=='n' -->
                                                    <!-- ngIf: $ctrl.license.content.adaptation=='y' -->
                                                    <!-- ngIf: $ctrl.license.content.adaptation=='sa' -->
                                                    <li ng-if="$ctrl.license.content.adaptation=='sa'" class="ng-scope">
                                                        允许复制、发行演绎作品，但演绎作品必须采用与本协议相同或兼容的协议进行许可
                                                    </li><!-- end ngIf: $ctrl.license.content.adaptation=='sa' -->
                                                    <!-- ngIf: $ctrl.licenseConfirm --> </ul>
                                            </div>
                                            <div class="cm ng-hide" ng-show="$ctrl.license.type=='cm'"> 本作品使用商业转载授权协议发布
                                                <ul class="details">
                                                    <!-- ngIf: $ctrl.license.content.adaptation=='n' -->
                                                    <!-- ngIf: $ctrl.license.content.adaptation=='y' --> </ul>
                                            </div>
                                        </div>
                                    </div>
                                </license>
                            </div>
                        </div>
                    </publish-license>
                </div>
            </publish-article>
        </div>
        <div class="wrapper-footer">
            <div class="footer-div">
                <button class="btn" @click="publishArticle">发布</button>
            </div>
        </div>
    </div>

</template>

<script>
    export default {
        data () {
            return {
                note: {
                    title: '无标题文档'
                },
                show_result: false,
                editorOption: {},
                resMsg: '',
                adapt: 'sa',
                commercial: 'y'
            }
        },
        mounted () {
            this.$http.get('/user/isLogin').then(function (response) {
                if (response.body === '已登录') {
                    this.$store.dispatch('changeIsLogin', true)
                } else {
                    this.$store.dispatch('changeIsLogin', false)
                    this.$router.push('/login')
                }
            })
        },
        methods: {
            // 发布文章
            publishArticle () {
                var _self = this
                _self.resMsg = '恭喜你!发表成功'
                _self.show_result = true
                setTimeout(function () {
                    _self.$router.push('/home/article/hot')
                }, 1000)
            },
            // editor 初始化
            onEditorReady (e) {
            },
            // editor获取焦点
            onEditorFocus (e) {
            },
            // editor失去焦点
            onEditorBlur (e) {
            }
        }
    }
</script>

<style>
    .outer-container {
        height: calc(100% - 4px);
        width: 100%;
        background-color: #fff;
    }

    .publish-active {
        position: absolute;
        width: 240px;
        top: 300px;
        z-index: 10;
        box-shadow: 5px 5px 5px rgba(0, 0, 0, .6);;

    }

    .publish_result {
        margin: 0 auto;
        width: 240px;
    }

    .guide-wrapper {
        background-color: #f0f0f0;
        padding-top: 34px;
        padding-bottom: 14px;
    }

    .guide {
        background-color: #f0f0f0;
        position: relative;
        height: 45px;
        z-index: 1
    }

    .guide * {
        box-sizing: border-box;
    }

    .guide .step {
        position: relative;
        display: block;
        float: left;
        width: 33.3%;
        z-index: 2
    }

    .guide .step .dot-wrapper {
        position: relative;
        width: 24px;
        height: 12px;
        background-color: #f0f0f0;
        margin: 0 auto
    }

    .guide .step .dot-wrapper .dot {
        position: relative;
        width: 12px;
        height: 12px;
        border-radius: 6px;
        border: 1px solid #ccc;
        margin: 0 auto
    }

    .guide .step.active .dot {
        border: 0;
        background-color: #1c8c79
    }

    .guide .step.active .text {
        color: #1c8c79
    }

    .guide .step .text {
        position: relative;
        margin-top: 13px;
        text-align: center;
        color: #ccc;
        font-size: 14px;
        font-family: PingFang SC, HanHei SC, Microsoft YaHei, Helvetica Neue, Helvetica, Arial, "sans-serif";
        line-height: 1.15;
        text-shadow: none;
        -webkit-font-smoothing: antialiased;
    }

    .guide .line {
        position: absolute;
        width: 66.7%;
        height: 1px;
        background-color: #ccc;
        top: 6px;
        left: 16.7%;
        z-index: 1
    }

    .page-content {
        position: relative;
        background-color: #fff;
        margin-top: 16px;
        z-index: 2;
        box-shadow: 0 0 1px 1px rgba(0, 0, 0, .03)
    }

    publish-article * {
        box-sizing: border-box;
    }

    .license .text {
        position: relative;
        display: block;
        margin-top: 25px;
        margin-left: 0;
        color: #787878;
        font-size: 14px;
        font-family: PingFang SC, HanHei SC, Microsoft YaHei, Helvetica Neue, Helvetica, Arial, "sans-serif";
        line-height: 1.15;
        text-shadow: none;
        -webkit-font-smoothing: antialiased;
    }

    .license .text .license-link {
        display: inline;
        color: #1c8c79;
        text-decoration: underline;
        background-color: transparent;
    }

    .license .text .details {
        padding-left: 20px;
        margin-top: 10px
    }

    .license .text ul {
        list-style-type: disc;
    }

    .license .text .details li {
        line-height: 1.5
    }

    license-icons {
        position: relative;
        display: block
    }

    license-icons .icons {
        height: 30px;
        line-height: 30px;
        margin-top: 20px
    }

    license-icons .icons a {
        display: inline-block;
        height: 30px;
        margin: 0 2px
    }

    license-icons .icons a img {
        width: 30px;
        height: 30px
    }

    license-icons.inline .icons {
        display: inline-block;
        vertical-align: middle;
        margin-top: 0
    }

    license-icons.small .icons {
        height: 16px;
        line-height: 16px
    }

    license-icons.small .icons a {
        height: 16px
    }

    license-icons.small .icons a img {
        width: 16px;
        height: 16px
    }

    license-icons.mini .icons {
        height: 12px;
        line-height: 12px
    }

    license-icons.mini .icons a {
        height: 12px
    }

    license-icons.mini .icons a img {
        width: 12px;
        height: 12px
    }

    publish-license .license-picker .license-type {
        padding: 55px 70px 0;
        border-bottom: 1px solid #cfcfcf
    }

    publish-license .license-picker .license-detail {
        padding: 34px 70px;
        border-bottom: 1px solid #cfcfcf
    }

    publish-license .license-picker .license-detail .question {
        position: relative;
        margin-top: 25px
    }

    publish-license .license-picker .license-detail .question label {
        margin-right: 100px;
        cursor: pointer
    }

    publish-license .license-picker .license-detail .license-item:not(:first-child) {
        margin-top: 52px
    }

    publish-license .license-picker .license-preview {
        padding: 34px 70px
    }

    publish-license .license-picker .license-preview .name {
        font-size: 16px;
        color: #2b2b2b
    }

    publish-license .license-picker .license-preview .license-icons a {
        display: block;
        width: 29px;
        height: 29px;
        margin-right: 20px;
        float: left
    }

    publish-license .license-picker .license-preview .license-icons a img {
        display: block;
        width: 100%;
        height: 100%
    }

    publish-license .license-picker .description {
        font-size: 16px;
        color: #4b4b4b
    }

    publish-license .license-picker .options {
        position: relative;
        height: 130px;
        margin-top: 30px
    }

    publish-license .license-picker .options .option {
        position: relative;
        width: 50%;
        float: left;
        text-align: center;
        padding-bottom: 24px
    }

    publish-license .license-picker .options .option .icon {
        position: relative;
        height: 64px;
        cursor: pointer
    }

    publish-license .license-picker .options .option .icon img {
        width: 64px;
        height: 64px;
        opacity: .16
    }

    publish-license .license-picker .options .option label {
        display: block;
        cursor: pointer;
        margin-top: 20px;
        font-size: 16px;
        color: #2b2b2b
    }

    publish-license .license-picker .options .option .triangle {
        position: absolute;
        display: none;
        width: 15px;
        height: 15px;
        bottom: -11px;
        left: 50%;
        margin-left: -7px;
        border-left: 1px solid #cfcfcf;
        border-top: 1px solid #cfcfcf;
        background-color: #fff;
        transform: rotate(45deg)
    }

    publish-license .license-picker .options .option.active .triangle {
        display: block
    }

    publish-license .license-picker .options .option.active .icon img {
        opacity: 1
    }

    publish-license .license-picker .price-tag input {
        display: block;
        position: absolute;
        border: 0;
        margin: 0;
        padding: 0;
        font-size: 14px;
        color: #2b2b2b;
        height: 40px;
        line-height: 40px;
        text-align: center;
        width: 80px;
        top: 2px;
        left: 2px
    }

    publish-license .license-picker .price-tag .text {
        position: relative;
        margin-left: 83px
    }

    @media only screen and (max-width: 992px) {
        .price-tag input {
            width: 80px !important
        }
    }

    .guide .step.active .text {
        color: #1c8c79
    }

    .guide .step .text {
        position: relative;
        margin-top: 13px;
        text-align: center;
        color: #ccc;
        font-size: 14px
    }

    .page-content {
        position: relative;
        background-color: #fff;
        margin-top: 16px;
        z-index: 2;
        box-shadow: 0 0 1px 1px rgba(0, 0, 0, .03)
    }

    .wrapper-footer {
        margin: 0 auto;
    }

    .footer-div {
        margin: 0 auto;
        width: 100px;
    }

    .footer-div .btn {
        margin: 20px auto 15px auto;
        width: 100%;
        height: 30px;
        border: 0;
        border-radius: 4px;
        cursor: pointer;
        background-color: #049cdb;
        color: #fff;
    }
</style>
