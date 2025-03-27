class WaterQuality:
    def __init__(self, region, pH, DO, COD):
        """
        :param region: 지역명
        :param pH: pH 값 (수소이온농도)
        :param DO: 용존산소 (Dissolved Oxygen)
        :param COD: 화학적 산소요구량 (Chemical Oxygen Demand)
        """
        self.region = region
        self.pH = pH
        self.DO = DO
        self.COD = COD

    def check_pH(self):
        """pH 값에 따른 수질 평가"""
        if self.pH < 6.5:
            return "산성 (수질 나쁨)"
        elif 6.5 <= self.pH <= 8.5:
            return "정상 (수질 양호)"
        else:
            return "염기성 (수질 나쁨)"

    def check_DO(self):
        """용존산소(DO) 농도에 따른 수질 평가"""
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

    def get_quality_report(self):
        """수질 평가 보고서 출력"""
        return f"지역: {self.region}\n" \
               f"pH: {self.check_pH()}\n" \
               f"용존산소(DO): {self.check_DO()}\n" \
               f"화학적 산소요구량(COD): {self.check_COD()}"

if __name__ == "__main__":
    region = input("지역명을 입력하세요: ")
    pH = float(input("pH 값을 입력하세요: "))
    DO = float(input("용존산소(DO) 값을 입력하세요 (mg/L): "))
    COD = float(input("화학적 산소요구량(COD) 값을 입력하세요 (mg/L): "))
   
    water_quality = WaterQuality(region, pH, DO, COD)
    print(water_quality.get_quality_report())
