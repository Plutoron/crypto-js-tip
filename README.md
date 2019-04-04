# crypto-js-tip
cryptoJs 使用中的 坑

### aes 

对应 node crypto 中的 createCipheriv 不兼容旧的 createCipher

```
    const data = 'your needed encrypt string'
    
    // 密钥长度,分别对应16字节,24字节,32字节
    // 对应 128 192 256 bits 加密
    // 可以 md5 快速生成 32 字节 密钥
    
    const key = '5334519cc2af4a5aa6fde4ac17aa01e3'
    
    // CryptoJS.AES.encrypt 加密内容 和 key 需要是 Utf8
    
		const encrypted = CryptoJS.AES.encrypt(Utf8.parse(data), Utf8.parse(key), {
			mode: CryptoJS.mode.ECB,
			padding: CryptoJS.pad.Pkcs7,
			iv: '',
		})
    const ciphertext = encrypted.ciphertext // wordArray
    
    // 需要 toString 保存为 二进制 或 base64 格式 的 密文
    
    const hexCiphertext = ciphertext.toString() // toString 默认 二进制
    const base64Ciphertext = ciphertext.toString(Base64) 
    
		console.log('ciphertext', ciphertext)
		console.log('ciphertext hex', hexCiphertext)
		console.log('ciphertext base64', base64Ciphertext)
    
		// CryptoJS.AES.decrypt 密文 需要是 base64 
    
    // 对应 hex 密文 解密，先将 hex 恢复 再 进行 base64，key 也需 Utf8
		// const decrypt = CryptoJS.AES.decrypt(Base64.stringify(Hex.parse(test)), Utf8.parse(key), {
		// 	mode: CryptoJS.mode.ECB,
		// 	padding: CryptoJS.pad.Pkcs7,
		// 	iv: '',
		// })

    // 对用 base64 密文 解密
		const decrypt = CryptoJS.AES.decrypt(base64Ciphertext, Utf8.parse(key), {
			mode: CryptoJS.mode.ECB,
			padding: CryptoJS.pad.Pkcs7,
			iv: '',
		})
    
    console.log('decrypt', decrypt.toString(Utf8))
```
