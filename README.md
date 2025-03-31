class WaterQuality:
    def __init__(self, region, pH, DO, COD):
        self.region = region
        self.pH = pH
        self.DO = DO
        self.COD = COD

    def check_pH(self):
        if self.pH < 6.5:
            return "산성 (수질 나쁨)"
        elif 6.5 <= self.pH <= 8.5:
            return "정상 (수질 양호)"
        else:
            return "염기성 (수질 나쁨)"

    def check_DO(self):
        if self.DO < 4.0:
            return "산소 부족 (수질 나쁨)"
        elif 4.0 <= self.DO <= 8.0:
            return "정상 (수질 양호)"
        else:
            return "과도한 산소 (이상적이지 않음)"

    def check_COD(self):
        if self.COD > 10:
            return "높은 COD (수질 오염됨)"
        else:
            return "정상 (수질 양호)"
    
    def overall_quality(self):
        bad_factors = 0
        
        if "나쁨" in self.check_pH():
            bad_factors += 1
        if "나쁨" in self.check_DO() or "이상적이지 않음" in self.check_DO():
            bad_factors += 1
        if "오염됨" in self.check_COD():
            bad_factors += 1
        
        if bad_factors == 0:
            return "좋음 (수질 매우 양호)"
        elif bad_factors == 1:
            return "보통 (수질 준수)"
        else:
            return "나쁨 (수질 개선 필요)"
    
    def get_quality_report(self):
        return f"지역: {self.region}\n" \
               f"pH: {self.check_pH()}\n" \
               f"용존산소(DO): {self.check_DO()}\n" \
               f"화학적 산소요구량(COD): {self.check_COD()}\n" \
               f"종합 평가: {self.overall_quality()}"

if __name__ == "__main__":
    region = input("지역명을 입력하세요: ")
    pH = float(input("pH 값을 입력하세요: "))
    DO = float(input("용존산소(DO) 값을 입력하세요 (mg/L): "))
    COD = float(input("화학적 산소요구량(COD) 값을 입력하세요 (mg/L): "))
   
    water_quality = WaterQuality(region, pH, DO, COD)
    print(water_quality.get_quality_report())
