import Array "mo:base/Array";

actor PetTracking {
    // Pet bilgilerini tutmak için public bir tip tanımlıyoruz
    public type PetInfo = {
        name: Text;
        type_: Text;
        age: Int;
        healthStatus: Text;
        owner: Text;
        vaccinationRecords: [Text]; 
        vetVisits: [Text];
        activities: [Text];  
    };

    // Dahili kullanım için mutable Pet tipi
    private type Pet = {
        var name: Text;
        var type_: Text;
        var age: Int;
        var healthStatus: Text;
        var owner: Text;
        var vaccinationRecords: [Text]; 
        var vetVisits: [Text];
        var activities: [Text];  
    };

    private var pets: [var Pet] = [var];

    public func addPet(name: Text, type_: Text, age: Int, owner: Text) : async Text {
        let newPet: Pet = {
            var name = name;
            var type_ = type_;
            var age = age;
            var healthStatus = "Sağlıklı"; 
            var owner = owner;
            var vaccinationRecords = [];
            var vetVisits = [];
            var activities = [];
        };
        
        pets := Array.thaw(Array.append(Array.freeze(pets), [newPet]));
        return "Evcil hayvan başarıyla eklendi!";
    };

    public func updateHealthStatus(petName: Text, newHealthStatus: Text) : async Text {
        for (pet in pets.vals()) {
            if (pet.name == petName) {
                pet.healthStatus := newHealthStatus;
                return "Sağlık durumu güncellendi!";
            };
        };
        return "Evcil hayvan bulunamadı!";
    };

    public func addVaccinationRecord(petName: Text, vaccination: Text) : async Text {
        for (pet in pets.vals()) {
            if (pet.name == petName) {
                pet.vaccinationRecords := Array.append(pet.vaccinationRecords, [vaccination]);
                return "Aşı kaydı başarıyla eklendi!";
            };
        };
        return "Evcil hayvan bulunamadı!";
    };

    public func addVetVisit(petName: Text, visitDetails: Text) : async Text {
        for (pet in pets.vals()) {
            if (pet.name == petName) {
                pet.vetVisits := Array.append(pet.vetVisits, [visitDetails]);
                return "Veteriner ziyareti kaydedildi!";
            };
        };
        return "Evcil hayvan bulunamadı!";
    };

    public func addActivity(petName: Text, activity: Text) : async Text {
        for (pet in pets.vals()) {
            if (pet.name == petName) {
                pet.activities := Array.append(pet.activities, [activity]);
                return "Aktivite kaydedildi!";
            };
        };
        return "Evcil hayvan bulunamadı!";
    };

    public query func getPetDetails(petName: Text) : async ?PetInfo {
        for (pet in pets.vals()) {
            if (pet.name == petName) {
                // Dahili Pet tipini public PetInfo tipine dönüştürüyoruz
                let petInfo: PetInfo = {
                    name = pet.name;
                    type_ = pet.type_;
                    age = pet.age;
                    healthStatus = pet.healthStatus;
                    owner = pet.owner;
                    vaccinationRecords = pet.vaccinationRecords;
                    vetVisits = pet.vetVisits;
                    activities = pet.activities;
                };
                return ?petInfo;
            };
        };
        return null;
    };
};
