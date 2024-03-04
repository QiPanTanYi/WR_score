# WR_score-脚本使用说明书(该项目已废除)
​        此脚本主要是用来刷微软的搜索积分，采用了ES6语法，可以支持移动端及PC端浏览器的微软搜索类积分获取，早日兑换奖品！

主文档：[TanZ的薅羊毛攻略-微软积分 (qipantanyi.github.io)](https://qipantanyi.github.io/WR_score/)

PC端浏览器脚本部署：[PC端部署-微软积分 (qipantanyi.github.io)](https://qipantanyi.github.io/WR_score/page/page01.html)

移动端 x浏览器脚本部署：[移动端部署-微软积分 (qipantanyi.github.io)](https://qipantanyi.github.io/WR_score/page/page02.html)

微软积分商城：[Microsoft Rewards (bing.com)](https://rewards.bing.com/)

Bing搜索页面：[必应 (bing.com)](https://cn.bing.com/)



#### 脚本内容：

```javascript
// ==UserScript==
// @name        自动刷微软积分脚本
// @namespace   Violentmonkey Scripts
// @match       https://*.bing.com/*
// @grant       none
// @version     1.0
// @author      TanZ
// @description PC端及移动端浏览器刷微软积分脚本
// ==/UserScript==

const searchUrl = 'https://www.bing.com/search';
const isDesktop = window.matchMedia("(min-width: 768px)").matches;

// 检查当前URL是bing.com还是cn.bing.com
if (window.location.hostname.endsWith('.bing.com')) {
       // 每2000毫秒(2秒)执行一次随机搜索
       // 根据设备类型设置循环次数（PC浏览器36次，bing浏览器APP30次）
    const loopCount = isDesktop ? 36 : 30; 
    let count = localStorage.getItem('searchCount') || 0;    // 从存储器中获取计数器初始值
    const intervalId = setInterval(() => {
        if (count >= loopCount) {
            clearInterval(intervalId);  // 停止循环
            localStorage.removeItem('searchCount');  // 删除计数器的值
            window.close(); // 关闭 Bing 页面
            return;
        }
        // 生成随机字符填入搜索框
        const search = generateRandomSearch();

        // 在必应搜索栏中输入搜索查询
        const searchBox = document.getElementById('sb_form_q');
        searchBox.value = search;
        searchBox.dispatchEvent(new Event('input'));
        // 点击提交按钮
        const searchForm = document.getElementById('sb_form');
        searchForm.submit();
        // 增加计数器并更新存储中的值
        count++;
        localStorage.setItem('searchCount', count);
    }, 2000);
}

function generateRandomSearch() {
    let search = '';
    // 生成由4位数字和1个字母组成的随机字符串
    for (let i = 0; i < 4; i++) {
        search += Math.floor(Math.random() * 10);
    }
    search += String.fromCharCode(Math.floor(Math.random() * 26) + 65);
    return search;
}

```

