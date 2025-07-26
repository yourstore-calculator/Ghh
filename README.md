# Ghh
import math

# =============================================
# دوال مساعدة
# =============================================

def get_dimensions(is_per_meter=True):
    """أخذ العرض والطول من المستخدم وحساب المساحة"""
    while True:
        try:
            if is_per_meter:
                print("\n📏 الرجاء إدخال الأبعاد بالمتر:")
            width = float(input("العرض (م): "))
            height = float(input("الطول/الارتفاع (م): "))
            if width <= 0 or height <= 0:
                print("❌ الأبعاد يجب أن تكون موجبة. حاول مجددًا.")
                continue
            return width, height, (width * height)
        except ValueError:
            print("❌ أدخل أرقام فقط.")

def calculate_door_extra_charge(area, standard_area=2.2):
    """حساب تكلفة إضافية للأبواب التي تزيد عن الحجم القياسي"""
    extra_charge = 0
    if area > standard_area:
        increments = math.ceil((area - standard_area) / 0.1)
        extra_charge = increments * 2
        print(f"💡 تمت إضافة {extra_charge} ريال بسبب زيادة المساحة.")
    return extra_charge

def get_quantity():
    """أخذ الكمية من المستخدم"""
    while True:
        try:
            qty = int(input("🔢 أدخل الكمية: "))
            if qty <= 0:
                print("❌ الكمية يجب أن تكون أكبر من صفر.")
                continue
            return qty
        except ValueError:
            print("❌ يرجى إدخال رقم صحيح.")

def get_curtain_area():
    """إدخال مقاسات الستارة"""
    print("\n🪟 إدخال مقاسات الستارة الداخلية:")
    width = float(input("عرض الستارة (م): "))
    height = float(input("ارتفاع الستارة (م): "))
    return width * height

def print_result(price, cbm, quantity):
    """طباعة النتيجة النهائية مع الشحن والكمية"""
    shipping = cbm * 48
    total = (price + shipping) * quantity
    print("\n" + "="*30)
    print("      ✅ النتيجة النهائية ✅")
    print("="*30)
    print(f"📦 الحجم (CBM): {cbm:.3f}")
    print(f"🚚 تكلفة الشحن: {shipping:.2f} ريال")
    print(f"🧾 السعر قبل الشحن: {price:.2f} ريال")
    print(f"🔢 الكمية: {quantity}")
    print(f"💰 السعر النهائي: {total:.2f} ريال عماني")
    print("="*30 + "\n")


# =============================================
# دوال الحسابات
# =============================================

def calculate_windows():
    print("""
اختر نوع النافذة:
1. دبل جلاس دبل فريم
2. دبل جلاس سنجل فريم
3. سنجل جلاس سنجل فريم
4. نوافذ سلايدنج
5. نوافذ كهربائية
""")
    choice = input("ادخل رقم النوع (1-5): ")

    rate = 0
    cbm_factor = 0.13
    is_double = False

    if choice == '1':
        is_double = True
        move = input("الحركة (1: ثابتة، 2: حركة، 3: حركتين): ")
        rates = {'1': 34, '2': 73, '3': 92}
        rate = rates.get(move, 34)

    elif choice == '2':
        is_double = True
        move = input("الحركة (1: ثابتة، 2: حركة، 3: حركتين): ")
        rates = {'1': 26, '2': 46, '3': 58}
        rate = rates.get(move, 26)

    elif choice == '3':
        move = input("الحركة (1: ثابتة، 2: حركة، 3: حركتين): ")
        rates = {'1': 20, '2': 43, '3': 47}
        rate = rates.get(move, 20)

    elif choice == '4':
        base_rate = 58
        _, _, area = get_dimensions()
        price = (base_rate * area) + 10
        cbm = area * 0.13
        quantity = get_quantity()
        print_result(price, cbm, quantity)
        return

    elif choice == '5':
        rate = 102
        cbm_factor = 0.13
        is_double = True

    else:
        print("❌ خيار غير صحيح.")
        return

    _, _, area = get_dimensions()
    price = rate * area
    cbm = cbm_factor * area

    # إضافات: الشبك
    if input("هل تريد شبك؟ (نعم/لا): ").strip().lower() == 'نعم':
        print("نوع الشبك: 1. باب (39)  2. فولدنج (18)  3. سلايد (14)")
        mesh = input("اختر النوع: ")
        mesh_rates = {'1': 39, '2': 18, '3': 14}
        mesh_rate = mesh_rates.get(mesh, 14)
        mesh_area = 0.5 * area
        mesh_cost = mesh_rate * mesh_area
        price += mesh_cost
        print(f"🧩 تكلفة الشبك: {mesh_cost:.2f} ريال")

    # إضافات: الستارة
    if is_double and input("هل تريد ستارة داخلية؟ (نعم/لا): ").strip().lower() == 'نعم':
        curtain_area = get_curtain_area()
        curtain_cost = 26 * curtain_area
        price += curtain_cost
        print(f"🧩 تكلفة الستارة: {curtain_cost:.2f} ريال")

    quantity = get_quantity()
    print_result(price, cbm, quantity)


def calculate_doors():
    print("""
اختر نوع الباب:
1. باب المدخل
2. أبواب WPC
3. أبواب الألمنيوم
4. أبواب دورات المياه
5. أبواب السحب
6. أبواب الفولدنج
""")
    choice = input("ادخل اختيارك: ")

    if choice == '1':
        print("مادة الباب: 1. زينك 2. ستيل 3. كاست المنيوم")
        mat = input("اختر: ")
        rates = {'1': 66, '2': 120, '3': 168}
        rate = rates.get(mat, 66)
        _, _, area = get_dimensions()
        price = (rate * area) + 10
        cbm = area * 0.2

    elif choice == '2':
        print("نوع WPC: 1. فارغ 2. مع خشب 3. ضد الصوت 4. فريم ألمنيوم 5. سلايدنج")
        t = input("اختر: ")
        rates = {'1': 45, '2': 50, '3': 60, '4': 67, '5': 65}
        rate = rates.get(t, 45)
        w, h, area = get_dimensions()
        price = rate + calculate_door_extra_charge(area)
        cbm_factor = 0.11 if t == '5' else 0.11
        cbm = area * cbm_factor

        if input("تلبيسة للسقف؟ (نعم/لا): ").strip().lower() == 'نعم':
            length = float(input("طول التلبيسة (م): "))
            cost = math.ceil(length) * 10
            price += cost
            cbm = (h + length) * w * cbm_factor
            print(f"🧩 تكلفة التلبيسة: {cost:.2f} ريال")

    elif choice == '3':
        print("1. فارغ 2. مع خشب 3. فل ألمنيوم 4. مخفي 5. خارجي")
        t = input("اختر: ")
        rates = {'1': 65, '2': 75, '3': 85, '4': 110, '5': 61}
        rate = rates.get(t, 65)
        _, _, area = get_dimensions()
        price = rate + calculate_door_extra_charge(area)
        cbm = area * 0.11

    elif choice == '4':
        print("1. جديد (55)  2. قديم (45)  3. زجاجي مخفي (65)")
        t = input("اختر: ")
        rates = {'1': 55, '2': 45, '3': 65}
        rate = rates.get(t, 55)
        _, _, area = get_dimensions()
        price = rate + calculate_door_extra_charge(area, 0.22)
        cbm = area * 0.11

    elif choice == '5':
        print("1. داخلي زجاج 2. داخلي متين 3. خارجي جزء مفتوح 4. جزئين 5. WPC")
        t = input("اختر: ")
        rates = {'1': 38, '2': 41, '3': 55, '4': 58, '5': 61}
        rate = rates.get(t, 38)
        _, _, area = get_dimensions()
        price = rate * area
        cbm = area * (0.11 if t == '5' else 0.13)

        if input("ستارة داخلية؟ (نعم/لا): ").strip().lower() == 'نعم':
            curtain_area = get_curtain_area()
            curtain_cost = 26 * curtain_area
            price += curtain_cost
            print(f"🧩 تكلفة الستارة: {curtain_cost:.2f} ريال")

    elif choice == '6':
        print("1. داخلي (39)  2. خارجي (56)")
        t = input("اختر: ")
        rates = {'1': 39, '2': 56}
        rate = rates.get(t, 39)
        _, _, area = get_dimensions()
        price = rate * area
        cbm = area * 0.13

        if input("ستارة داخلية؟ (نعم/لا): ").strip().lower() == 'نعم':
            curtain_area = get_curtain_area()
            curtain_cost = 26 * curtain_area
            price += curtain_cost
            print(f"🧩 تكلفة الستارة: {curtain_cost:.2f} ريال")

    else:
        print("❌ خيار غير صحيح.")
        return

    quantity = get_quantity()
    print_result(price, cbm, quantity)


def calculate_simple_meter_based(product_name, choices, cbm_factor):
    print(f"\nاختر نوع {product_name}:")
    for k, (desc, r) in choices.items():
        print(f"{k}. {desc} ({r} ريال/م)")
    t = input("اختر: ")
    if t not in choices:
        print("❌ خيار غير صحيح.")
        return
    rate = choices[t][1]
    _, _, area = get_dimensions()
    price = rate * area
    cbm = area * cbm_factor
    quantity = get_quantity()
    print_result(price, cbm, quantity)


# =============================================
# البرنامج الرئيسي
# =============================================

def main():
    while True:
        print("""
===============================
🔧 حاسبة الأسعار والشحن
===============================
1. نوافذ
2. أبواب
3. سكاي لايت
4. كارتن وول
5. شتر خارجي
6. أبواب حدائق
7. حواجز
0. خروج
""")
        ch = input("اختر الفئة: ").strip()

        if ch == '1':
            calculate_windows()
        elif ch == '2':
            calculate_doors()
        elif ch == '3':
            calculate_simple_meter_based("سكاي لايت", {'1': ("بدون مكينة", 56), '2': ("مع مكينة", 145)}, 0.13)
        elif ch == '4':
            calculate_simple_meter_based("كارتن وول", {'1': ("ثقيل", 56), '2': ("خفيف", 45)}, 0.13)
        elif ch == '5':
            rate = 28
            _, _, area = get_dimensions()
            cbm = area * 0.2
            price = rate * area
            quantity = get_quantity()
            print_result(price, cbm, quantity)
        elif ch == '6':
            rate = 91
            _, _, area = get_dimensions()
            cbm = area * 0.2
            price = rate * area
            quantity = get_quantity()
            print_result(price, cbm, quantity)
        elif ch == '7':
            calculate_simple_meter_based("الحواجز", {'1': ("حاجز درج", 43), '2': ("حاجز حمام", 32)}, 0.05)
        elif ch == '0':
            print("👋 شكراً لاستخدامك الحاسبة.")
            break
        else:
            print("❌ خيار غير صحيح.")

        input("🔁 اضغط Enter لإجراء عملية جديدة...")


if __name__ == "__main__":
    main()
