import base64

def xrule_encrypt(text: str) -> str:
    base64_encoded = base64.b64encode(text.encode('utf-8')).decode('utf-8')
    xrule_encoded = (
        base64_encoded
        .replace('+', '_')
        .replace('/', '-')
        .replace('=', '.')
    )
    return xrule_encoded

def xrule_decrypt(xrule_text: str) -> str:
    base64_text = (
        xrule_text
        .replace('_', '+')
        .replace('-', '/')
        .replace('.', '=')
    )
    try:
        decoded = base64.b64decode(base64_text).decode('utf-8')
        return decoded
    except Exception as e:
        return f"[解密失败] {e}"

# 示例用法
if __name__ == "__main__":
    original = ""
    
    encrypted = xrule_encrypt(original)
    print("加密后：", encrypted)

    decrypted = xrule_decrypt(encrypted)
    print("解密后：", decrypted)
