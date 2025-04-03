def safe_float_input(prompt):
    while True:
        try:
            return float(input(prompt))
        except ValueError:
            print("잘못된 입력입니다. 숫자를 입력하세요.")

def safe_list_float_input(prompt):
    while True:
        try:
            return list(map(float, input(prompt).split()))
        except ValueError:
            print("잘못된 입력입니다. 숫자를 공백으로 구분하여 입력하세요.")

class FactoryWastewaterMonitor:
    def __init__(self, factory_name, pH, COD, heavy_metal):
        self.factory_name = factory_name
        self.pH = pH
        self.COD = COD
        self.heavy_metal = heavy_metal
    
    def check_pH(self):
        if self.pH < 6.0:
            return "산성 폐수 (기준 초과 경고)"
        elif 6.0 <= self.pH <= 9.0:
            return "정상 범위"
        else:
            return "염기성 폐수 (기준 초과 경고)"
    
    def check_COD(self):
        return "COD 초과 (오염 위험)" if self.COD > 50 else "정상 범위"
    
    def check_heavy_metal(self):
        return "중금속 농도 초과 (위험)" if self.heavy_metal > 0.5 else "정상 범위"
    
    def get_wastewater_report(self):
        return f"공장명: {self.factory_name}\n" \
               f"pH: {self.check_pH()}\n" \
               f"화학적 산소요구량(COD): {self.check_COD()}\n" \
               f"중금속 농도: {self.check_heavy_metal()}"

if __name__ == "__main__":
    factory_name = input("공장명을 입력하세요: ")
    pH = safe_float_input("pH 값을 입력하세요: ")
    COD = safe_float_input("화학적 산소요구량(COD) 값을 입력하세요 (mg/L): ")
    heavy_metal = safe_float_input("중금속 농도 값을 입력하세요 (mg/L): ")
    
    wastewater_monitor = FactoryWastewaterMonitor(factory_name, pH, COD, heavy_metal)
    print(wastewater_monitor.get_wastewater_report())
