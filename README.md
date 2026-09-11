# SYSTEMS-OF-LINEAR-EQUATIONS
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
