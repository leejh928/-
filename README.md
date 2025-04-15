def safe_float_input(prompt):
    while True:
        try:
            return float(input(prompt))
        except ValueError:
            print("숫자로 다시 입력해주세요.")

class WastewaterMonitor:
    def __init__(self, name, industry, pH, COD, heavy_metal, SS, TN, TP):
        self.name = name
        self.industry = industry
        self.pH = pH
        self.COD = COD
        self.heavy_metal = heavy_metal
        self.SS = SS
        self.TN = TN
        self.TP = TP
        self.standards = self.get_standards_by_industry()

    def get_standards_by_industry(self):
        if self.industry == "식품":
            return {
                "pH": (5.5, 8.5),
                "COD": 120,
                "heavy_metal": 0.2,
                "SS": 40,
                "TN": 20,
                "TP": 2
            }
        elif self.industry == "도금":
            return {
                "pH": (6.0, 9.0),
                "COD": 50,
                "heavy_metal": 0.5,
                "SS": 30,
                "TN": 15,
                "TP": 1
            }
        elif self.industry == "제지":
            return {
                "pH": (6.0, 8.5),
                "COD": 100,
                "heavy_metal": 0.3,
                "SS": 50,
                "TN": 25,
                "TP": 2
            }
        elif self.industry == "공공폐수":
            return {
                "pH": (5.8, 8.6),
                "COD": 40,
                "heavy_metal": 0.3,
                "SS": 20,
                "TN": 10,
                "TP": 0.5
            }
        else:
            raise ValueError("지원하지 않는 업종입니다.")

    def check(self, value, low, high):
        if value < low or value > high:
            return "위험", 2
        return "정상", 0

    def check_limit(self, value, limit):
        if value > limit:
            return "주의", 1
        return "정상", 0

    def get_report(self):
        total_score = 0
        s = self.standards

        pH_result, s1 = self.check(self.pH, s["pH"][0], s["pH"][1])
        cod_result, s2 = self.check_limit(self.COD, s["COD"])
        hm_result, s3 = self.check_limit(self.heavy_metal, s["heavy_metal"])
        ss_result, s4 = self.check_limit(self.SS, s["SS"])
        tn_result, s5 = self.check_limit(self.TN, s["TN"])
        tp_result, s6 = self.check_limit(self.TP, s["TP"])

        total_score = s1 + s2 + s3 + s4 + s5 + s6

        if total_score <= 2:
            grade = "양호"
        elif total_score <= 5:
            grade = "주의"
        else:
            grade = "심각"

        return f"""
=== {self.name} 공장 ({self.industry} 업종) 폐수 평가 ===
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
    print("공장 폐수 업종별 평가 프로그램")

    name = input("공장명을 입력하세요: ")
    print("업종을 선택하세요 (식품 / 도금 / 제지 / 공공폐수)")
    industry = input("업종 입력: ").strip()

    pH = safe_float_input("pH 값을 입력하세요: ")
    COD = safe_float_input("COD 값을 입력하세요 (mg/L): ")
    heavy_metal = safe_float_input("중금속 값을 입력하세요 (mg/L): ")
    SS = safe_float_input("SS 값을 입력하세요 (mg/L): ")
    TN = safe_float_input("총질소 값을 입력하세요 (mg/L): ")
    TP = safe_float_input("총인 값을 입력하세요 (mg/L): ")

    monitor = WastewaterMonitor(name, industry, pH, COD, heavy_metal, SS, TN, TP)
    print(monitor.get_report())

