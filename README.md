def calculate_progressive_tax(net_income):
    """คำนวณภาษีแบบขั้นบันไดตามตารางปี 2569"""
    brackets = [
        (150000, 0.00),
        (300000, 0.05),
        (500000, 0.10),
        (750000, 0.15),
        (1000000, 0.20),
        (2000000, 0.25),
        (5000000, 0.30),
        (float('inf'), 0.35)
    ]
    
    tax = 0.0
    previous_limit = 0
    breakdown = []
    
    for limit, rate in brackets:
        if net_income > previous_limit:
            taxable_in_bracket = min(net_income, limit) - previous_limit
            tax_in_bracket = taxable_in_bracket * rate
            tax += tax_in_bracket
            if rate > 0 and taxable_in_bracket > 0:
                breakdown.append({
                    "range": f"{previous_limit+1:,.0f} - {min(net_income, limit):,.0f}",
                    "rate": f"{int(rate*100)}%",
                    "amount": taxable_in_bracket,
                    "tax": tax_in_bracket
                })
            previous_limit = limit
        else:
            break
            
    return tax, breakdown

def calculate_tax_40_8(income, expense_type='lump_sum', actual_expense=0, deductions_dict=None):
    """
    คำนวณภาษี 40(8) สำหรับพ่อค้าแม่ค้า
    - income: รายได้รวมทั้งปี
    - expense_type: 'lump_sum' (เหมา 60%) หรือ 'actual' (ตามจริง)
    - actual_expense: ค่าใช้จ่ายตามจริง (ใช้กรณี expense_type='actual')
    - deductions_dict: Dictionary รวมรายการลดหย่อนต่างๆ
    """
    if deductions_dict is None:
        deductions_dict = {}
        
    # 1. คำนวณค่าใช้จ่าย
    if expense_type == 'lump_sum':
        expense = income * 0.60
    else:
        expense = actual_expense
        
    income_after_expense = max(0, income - expense)
    
    # 2. คำนวณค่าลดหย่อน
    # ค่าลดหย่อนส่วนตัว (อัตโนมัติ 60,000)
    total_deductions = 60000
    
    # ประกันชีวิต + สุขภาพรวม ไม่เกิน 100,000
    life_ins = deductions_dict.get('life_insurance', 0)
    health_ins = min(deductions_dict.get('health_insurance', 0), 25000)
    combined_life_health = min(life_ins + health_ins, 100000)
    
    # รวมค่าลดหย่อนอื่นๆ
    total_deductions += combined_life_health
    total_deductions += min(deductions_dict.get('parent_health_insurance', 0), 15000)
    total_deductions += min(deductions_dict.get('social_security', 0), 10500)
    total_deductions += deductions_dict.get('spouse', 0)
    total_deductions += deductions_dict.get('children', 0)
    total_deductions += deductions_dict.get('parents', 0)
    total_deductions += deductions_dict.get('disabled', 0)
    total_deductions += min(deductions_dict.get('home_loan_interest', 0), 100000)
    total_deductions += min(deductions_dict.get('solar_rooftop', 0), 200000)
    
    # กลุ่มการลงทุนและเกษียณ (จำกัดรวมตามกฎหมาย)
    thaiesg = min(deductions_dict.get('thaiesg', 0), income * 0.30, 300000)
    rmf = min(deductions_dict.get('rmf', 0), income * 0.30, 500000)
    pension_ins = min(deductions_dict.get('pension_insurance', 0), income * 0.15, 200000)
    
    retirement_group = rmf + pension_ins + deductions_dict.get('pvd', 0) + deductions_dict.get('nsf', 0)
    retirement_group_capped = min(retirement_group, 500000)
    
    total_deductions += thaiesg + retirement_group_capped
    
    # คำนวณเงินได้สุทธิ
    net_income = max(0, income_after_expense - total_deductions)
    
    # 3. คำนวณภาษี
    tax_payable, tax_breakdown = calculate_progressive_tax(net_income)
    
    return {
        "gross_income": income,
        "expense": expense,
        "income_after_expense": income_after_expense,
        "total_deductions": total_deductions,
        "net_income": net_income,
        "tax_payable": tax_payable,
        "tax_breakdown": tax_breakdown
    }

# --- ตัวอย่างการใช้งาน ---
if __name__ == "__main__":
    # สมมติพ่อค้าออนไลน์มีรายได้ 1,200,000 บาท/ปี เลือกหักเหมา 60%
    # มีลดหย่อน: ประกันชีวิต 30,000 บาท, ซื้อ RMF 50,000 บาท, ดอกเบี้ยบ้าน 40,000 บาท
    user_deductions = {
        'life_insurance': 30000,
        'rmf': 50000,
        'home_loan_interest': 40000
    }
    
    result = calculate_tax_40_8(
        income=1200000,
        expense_type='lump_sum',
        deductions_dict=user_deductions
    )
    
    print(f"รายได้รวม: {result['gross_income']:,.2f} บาท")
    print(f"หักค่าใช้จ่าย (60%): {result['expense']:,.2f} บาท")
    print(f"หักค่าลดหย่อนรวม: {result['total_deductions']:,.2f} บาท")
    print(f"เงินได้สุทธิ: {result['net_income']:,.2f} บาท")
    print(f"ภาษีที่ต้องชำระ: {result['tax_payable']:,.2f} บาท")
