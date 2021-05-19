# 3I_ChumpurovaI
import UIKit
// ДЗ - 1 и ДЗ-3. Описать несколько структур – любой легковой автомобиль SportCar и любой грузовик TrunkCar.
enum CarEngineState: String {
    case on = "Двигатель запущен"
    case off = "Двигатель выключен"
}
enum CarWindowsState: String {
    case open = "Окна открыты"
    case close = "Окна закрыты"
}
enum CarRiderName: String {
    case rai = "Kimi Raikkonen"
    case ham = "Lewis Hamilton"
    case bot = "Valtteri Bottas"
}
enum CarMakeName: String {
    case Mercedes, Ferrari, Mclaren
}
/*enum CarFilledBodVol: Int {
    case 10, 20, 30, 40, 50 (почему не получается так написать?)
} */

// Создание структуры для болида.
struct SportCar {
    let carMake: CarMakeName
    var yearOfIssue: Int
    let engineCapacity: Double = 1.6
    var engineState: CarEngineState {
        willSet {
            if newValue == .on {
                print("Внимание! Двигатель будет запущен.")
            } else {
                print("""
                      Двигатель будет выключен.
                      
                      """)
            }
        }
        didSet {
            if oldValue == .on {
                print("Двигатель выключен.")
            } else {
                print("Двигатель запущен.")
            }
        }
    }
    var windowsState: CarWindowsState
    var riderName: CarRiderName {
        didSet {
            print("""
                  На болид сел новый другой пилот - \(riderName.rawValue)

                  """)
    }
    }
    
    // Поменять пилота.
    mutating func changeRider(newRider: CarRiderName) {
        riderName = newRider
    }
    
    // Включить либо выключить двигатель.
    mutating func engineStateOn() {
        self.engineState = .on
    }
    mutating func engineStateOff() {
        self.engineState = .off
    }
    
    // Отобразить характеристики болида:
    func printCharacteristics() {
        print("""
              Марка машины: \(carMake.rawValue)
              Год выпуска: \(yearOfIssue)
              Объем двигателя: \(engineCapacity)
              Состояние двигателя: \(engineState.rawValue)
              \(windowsState.rawValue)
              Имя пилота: \(riderName.rawValue)
              
              """
        )
    }
}
var mcLarenCar = SportCar(carMake: .Mclaren, yearOfIssue: 2019, engineState: .on, windowsState: .open, riderName: .rai)
var mercedesSportCar = SportCar(carMake: .Mercedes, yearOfIssue: 2021, engineState: .on, windowsState: .close, riderName: .ham)
var ferrariSportCar = SportCar(carMake: .Ferrari, yearOfIssue: 1997, engineState: .off, windowsState: .close, riderName: .rai)

// ДЗ - 5 и ДЗ - 6.
mcLarenCar.printCharacteristics()
mcLarenCar.changeRider(newRider: .bot)
ferrariSportCar.engineStateOn()
ferrariSportCar.printCharacteristics()


    
struct TrunkCar {
    let carMake: String
    var yearOfIssue: Int
    let bodyVolume: Double
    var engineState: CarEngineState
    var windowsState: CarWindowsState
    var filledBodyVol: Double
    
    init(carMake: String, yearOfIssue: Int, bodyVolume: Double, engineState: CarEngineState, windowsState: CarWindowsState,filledBodyVolume: Double){
        self.carMake = carMake
        self.yearOfIssue = yearOfIssue
        self.bodyVolume = bodyVolume
        self.engineState = engineState
        self.windowsState = windowsState
        filledBodyVol = filledBodyVolume
        }
// ДЗ - 4, 5, 6. Добавить в структуры метод с одним аргументом типа перечисления, который будет менять свойства структуры в зависимости от действия. Инициализировать несколько экземпляров структур. Применить к ним различные действия. Вывести значения свойств экземпляров в консоль.

    // Открыть либо закрыть окна:
    mutating func closeWindow() {
        self.windowsState = .close
        print("У автомобиля марки " +  carMake + " " + windowsState.rawValue)
    }
    mutating func openWindow() {
        self.windowsState = .open
        print("У автомобиля марки " +  carMake + " " + windowsState.rawValue)
    }
    
    // Добавить либо выгрузить груз:
    mutating func addCArgo(to: Double) {
        if to < to + self.filledBodyVol || to < self.bodyVolume {
            filledBodyVol += to
            let remainder = bodyVolume - filledBodyVol
            print("После загрузки груза объемом \(to) общий объем груза в кузове составляет \(filledBodyVol). Осталось заполнить на: \(remainder)")
        } else {
            fatalError("Невозможно загрузить груз так как недостаточно места в кузове")
        }
    }
    mutating func unloadCargo(to: Double) {
        if to <= self.filledBodyVol {
            filledBodyVol -= to
            let remainder = bodyVolume - filledBodyVol
            print("После выгрузки груза объемом \(to) общий объем груза в кузове составляет \(filledBodyVol). Осталось заполнить на: \(remainder)")
        } else {
            fatalError("Невозможно выгрузить груз, потому что Вы хотите вытащить больше, чем там есть. Там есть всего лишь: \(filledBodyVol)")
        }
    }
    
    // Отобразить характеристики:
    func printCharacteristics() {
        print("""
            Марка автомобиля: \(carMake)
            Год выпуска: \(yearOfIssue)
            Объем кузова: \(bodyVolume)
            Состояние двигателя: \(engineState.rawValue)
            \(windowsState.rawValue)
            Объем груза, находящегося в кузове: \(filledBodyVol)
            """
        )
    }
    

}

var catTrunkCar = TrunkCar(carMake: "CAT", yearOfIssue: 2010, bodyVolume: 50, engineState: .on, windowsState: .close, filledBodyVolume: 20)

catTrunkCar.closeWindow()
catTrunkCar.openWindow()
catTrunkCar.addCArgo(to: 30)
catTrunkCar.printCharacteristics()
catTrunkCar.unloadCargo(to: 60)

