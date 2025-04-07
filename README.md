def safe_float_input(prompt):
    while True:
        try:
            return float(input(prompt))
        except ValueError:
            print("숫자로 다시 입력해주세요.")

class SimpleWastewaterMonitor:
    def __init__(self, name, pH, COD, heavy_metal, SS, TN, TP):
        self.name = name
        self.pH = pH
        self.COD = COD
        self.heavy_metal = heavy_metal
        self.SS = SS
        self.TN = TN
        self.TP = TP

    def check(self, value, low=None, high=None):
        if low is not None and value < low:
            return "위험", 2
        if high is not None and value > high:
            return "위험", 2
        return "정상", 0

    def check_limit(self, value, limit, level="주의"):
        if value > limit:
            return level, 2 if level == "위험" else 1
        return "정상", 0

    def get_report(self):
        total_score = 0

        pH_result, s1 = self.check(self.pH, 6.0, 9.0)
        cod_result, s2 = self.check_limit(self.COD, 50, "주의")
        hm_result, s3 = self.check_limit(self.heavy_metal, 0.5, "위험")
        ss_result, s4 = self.check_limit(self.SS, 30, "주의")
        tn_result, s5 = self.check_limit(self.TN, 20, "주의")
        tp_result, s6 = self.check_limit(self.TP, 2, "주의")

        total_score = s1 + s2 + s3 + s4 + s5 + s6

        if total_score <= 2:
            grade = "양호 "
        elif total_score <= 5:
            grade = "주의 ️"
        else:
            grade = "심각 "

        return f"""
=== {self.name} 공장 폐수 평가 ===
pH: {self.pH} → {pH_result}
COD: {self.COD} → {cod_result}
중금속: {self.heavy_metal} → {hm_result}
SS(부유물질): {self.SS} → {ss_result}
총질소(T-N): {self.TN} → {tn_result}
총인(T-P): {self.TP} → {tp_result}

>> 종합 점수: {total_score}
>> 오염 등급: {grade}
"""

if __name__ == "__main__":
    print("공장 폐수 간단 평가 프로그램")

    name = input("공장명을 입력하세요: ")
    pH = safe_float_input("pH 값을 입력하세요: ")
    COD = safe_float_input("COD 값을 입력하세요 (mg/L): ")
    heavy_metal = safe_float_input("중금속 값을 입력하세요 (mg/L): ")
    SS = safe_float_input("SS 값을 입력하세요 (mg/L): ")
    TN = safe_float_input("총질소 값을 입력하세요 (mg/L): ")
    TP = safe_float_input("총인 값을 입력하세요 (mg/L): ")

    monitor = SimpleWastewaterMonitor(name, pH, COD, heavy_metal, SS, TN, TP)
    print(monitor.get_report())
    
