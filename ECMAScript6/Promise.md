* 调用多个promise，同步的**先都**输出

* reject不影响后续输出，多个then情况 catch具有就近原则（输出最后一个reject参数内容）
* Finally 不论执行resolve/reject，最后都需要走一遍finally，避免重复代码
* Promise.all 传入Promise数组，多个Promise状态都改变，Promise.all才会改变（图片加载）
* Promise.race，一个Promise**率先**改变，Promise.all就改变，之后的其他Promise状态改变将不再响应。



五月君：

[https://www.nodejs.red/?nsukey=WfC9pA7bM2KY%2FfIgut7Ifp0Htg%2Fwl6l8J80lKtV%2FKLzz3O8NeY29yPFR7jr87LjZObzTJp3n%2FqZXAk7Apoj2LnidPCIyej%2FV0Bo3%2BgrL0tbx0qfxf01h2F9%2BO%2BBQY%2F4JTIvonr7I%2Fwcq0fQxSfXxFl6SUQ5G2pUmS8pHv%2BmUFaiMNOHd43K4S0qwnjBMCi0Q8AzgGD0pDRlpfaSO56JNPA%3D%3D#/es6/promise?id=%e9%94%99%e8%af%af%e6%8d%95%e8%8e%b7](https://www.nodejs.red/?nsukey=WfC9pA7bM2KY%2FfIgut7Ifp0Htg%2Fwl6l8J80lKtV%2FKLzz3O8NeY29yPFR7jr87LjZObzTJp3n%2FqZXAk7Apoj2LnidPCIyej%2FV0Bo3%2BgrL0tbx0qfxf01h2F9%2BO%2BBQY%2F4JTIvonr7I%2Fwcq0fQxSfXxFl6SUQ5G2pUmS8pHv%2BmUFaiMNOHd43K4S0qwnjBMCi0Q8AzgGD0pDRlpfaSO56JNPA%3D%3D#/es6/promise?id=错误捕获)