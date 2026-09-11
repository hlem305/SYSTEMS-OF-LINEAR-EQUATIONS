# SYSTEMS-OF-LINEAR-EQUATIONS
import numpy as np

def encrypt_message(message, key_matrix):
    """تشفير النص بضربه في مصفوفة المفتاح"""
    # تحويل النص إلى أرقام بناءً على ترتيب الحروف (A=1, B=2, إلخ، والفراغ=0)
    msg_numbers = [0 if char == ' ' else ord(char.upper()) - 64 for char in message]
    
    # حشو الرسالة بـ 0 إذا لم تكن مضاعفاً لحجم المصفوفة (هنا الحجم 2)
    if len(msg_numbers) % 2 != 0:
        msg_numbers.append(0)
        
    # تحويل القائمة المفرودة إلى مصفوفة بأعمدة من بكسلين (2xN)
    msg_matrix = np.array(msg_numbers).reshape(2, -1)
    
    # التشفير: ضرب مصفوفة المفتاح في مصفوفة الرسالة
    encrypted_matrix = np.dot(key_matrix, msg_matrix)
    return encrypted_matrix

def decrypt_message(encrypted_matrix, key_matrix):
    """فك تشفير المصفوفة بضربها في معكوس مصفوفة المفتاح"""
    # 1. حساب محدد المصفوفة (Determinant) ومعكوسها (Inverse) بالجبر الخطي
    det = np.linalg.det(key_matrix)
    if det == 0:
        raise ValueError("المصفوفة ليس لها معكوس، لا يمكن استخدامها للتشفير.")
        
    inv_key_matrix = np.linalg.inv(key_matrix)
    
    # 2. فك التشفير: ضرب المعكوس في المصفوفة المشفرة
    decrypted_matrix = np.dot(inv_key_matrix, encrypted_matrix)
    
    # تقريب الأرقام الناتجة لأقرب عدد صحيح وتحويلها مجدداً لنص
    decrypted_numbers = np.round(decrypted_matrix).flatten().astype(int)
    
    decrypted_text = ""
    for num in decrypted_numbers:
        if num == 0:
            decrypted_text += " "
        elif 1 <= num <= 26:
            decrypted_text += chr(num + 64)
            
    return decrypted_text.strip()

# --- تجربة الكود البرمجية ---
if __name__ == "__main__":
    # مصفوفة المفتاح السري (يجب أن يكون لها معكوس)
    secret_key = np.array([[2, 3], [1, 2]])
    message_to_send = "HELLO"
    
    # تشفير
    encrypted_data = encrypt_message(message_to_send, secret_key)
    # فك تشفير
    decrypted_data = decrypt_message(encrypted_data, secret_key)
    
    print("--- Ch02: مشروع تشفير الرسائل بالمصفوفات ---")
    print(f"النص الأصلي: {message_to_send}")
    print("المصفوفة المشفرة الناتجة:\n", encrypted_data)
    print(f"النص بعد فك التشفير: {decrypted_data}\n")

import numpy as np

def gauss_jordan_elimination(A, b):
    """
    يحل نظام المعادلات Ax = b باستخدام طريقة حذف غاوس-جوردان.
    """
    # دمج المصفوفة A مع متجه النواتج b لتكوين المصفوفة الممتدة (Augmented Matrix)
    augmented = np.hstack([A.astype(float), b.reshape(-1, 1).astype(float)])
    n = len(b)

    for i in range(n):
        # 1. جعل العنصر المحوري (Pivot) يساوي 1
        pivot = augmented[i, i]
        if pivot == 0:
            raise ValueError("المصفوفة ليس لها حل فريد (العنصر المحوري صفر).")
        augmented[i] = augmented[i] / pivot

        # 2. تصفيير باقي العناصر في نفس العمود فوق وتحت المحور
        for j in range(n):
            if i != j:
                factor = augmented[j, i]
                augmented[j] = augmented[j] - factor * augmented[i]

    # استخراج الحلول من العمود الأخير
    return augmented[:, -1]

# --- تجربة الكود البرمجية ---
if __name__ == "__main__":
    # حل نظام المعادلات:
    # 2x + y + z = 8
    # x + 3y + 2z = 13
    # x + y + 2z = 9
    A = np.array([[2, 1, 1], [1, 3, 2], [1, 1, 2]])
    b = np.array([8, 13, 9])

    solutions = gauss_jordan_elimination(A, b)
    print("--- Ch01: حل المعادلات بطريقة غاوس-جوردان ---")
    print(f"قيم المجاهيل (x, y, z) هي: {solutions}\n")
