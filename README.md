# Checkpoint-Mongoose-NodeJS-VS-MongoDB



// Importation des modules nécessaires  
const mongoose = require('mongoose'); // Importer Mongoose  
require('dotenv').config(); // Charger les variables d'environnement à partir du fichier .env  

// Connexion à la base de données MongoDB Atlas  
mongoose.connect(process.env.MONGO_URI, { useNewUrlParser: true, useUnifiedTopology: true })  
    .then(() => console.log('MongoDB Connected'))  
    .catch(err => console.error('Could not connect to MongoDB:', err));  

// Création du schéma de la personne  
const personSchema = new mongoose.Schema({  
    name: { type: String, required: true }, // Propriété name, chaîne de caractères, requise  
    age: Number, // Propriété age, nombre  
    favoriteFoods: [String] // Propriété favoriteFoods, tableau de chaînes de caractères  
});  

// Création du modèle Person à partir du schéma  
const Person = mongoose.model('Person', personSchema);  

// Création d'une instance de document  
const john = new Person({  
    name: 'John',  
    age: 25,  
    favoriteFoods: ['Pizza', 'Pasta']  
});  

// Sauvegarde de l'enregistrement dans la base de données  
john.save(function(err, data) {  
    if (err) return console.error(err);  
    console.log('Person saved:', data);  
});  

// Création de plusieurs personnes avec Model.create()  
const arrayOfPeople = [  
    { name: 'Mary', age: 30, favoriteFoods: ['Burritos', 'Tacos'] },  
    { name: 'Alice', age: 28, favoriteFoods: ['Salad', 'Sushi'] },  
    { name: 'Bob', age: 22, favoriteFoods: ['Steak', 'Fries'] }  
];  

Person.create(arrayOfPeople, function(err, data) {  
    if (err) return console.error(err);  
    console.log('Multiple people created:', data);  
});  

// Recherche de toutes les personnes par nom  
Person.find({ name: 'Mary' }, function(err, people) {  
    if (err) return console.error(err);  
    console.log('Found people with name Mary:', people);  
});  

// Recherche d'une personne par un aliment préféré  
Person.findOne({ favoriteFoods: 'Burritos' }, function(err, person) {  
    if (err) return console.error(err);  
    console.log('Found person with favorite food Burritos:', person);  
});  

// Recherche d'une personne par _id  
const personId = 'someObjectIdHere'; // Remplacez par un ObjectId valide  
Person.findById(personId, function(err, person) {  
    if (err) return console.error(err);  
    console.log('Found person by ID:', person);  
});  

// Mise à jour classique : Find, Edit, puis Save  
Person.findById(personId, function(err, person) {  
    if (err) return console.error(err);  
    person.favoriteFoods.push('hamburger'); // Ajoute 'hamburger' à la liste des aliments préférés  
    person.save(function(err, updatedPerson) {  
        if (err) return console.error(err);  
        console.log('Updated person:', updatedPerson);  
    });  
});  

// Mise à jour avec findOneAndUpdate()  
Person.findOneAndUpdate(  
    { name: 'Alice' }, // Critère de recherche  
    { age: 20 }, // Nouvelle valeur  
    { new: true }, // Retourner le document mis à jour  
    function(err, updatedPerson) {  
        if (err) return console.error(err);  
        console.log('Updated person age for Alice:', updatedPerson);  
    }  
);  

// Suppression d'une personne par _id  
Person.findByIdAndRemove(personId, function(err, removedPerson) {  
    if (err) return console.error(err);  
    console.log('Removed person:', removedPerson);  
});  

// Suppression de toutes les personnes portant le nom "Mary"  
Person.remove({ name: 'Mary' }, function(err, result) {  
    if (err) return console.error(err);  
    console.log('Number of people removed:', result.deletedCount);  
});  

// Recherche de personnes aimant les burritos  
Person.find({ favoriteFoods: 'burritos' })  
    .sort({ name: 1 }) // Tri par nom  
    .limit(2) // Limite à deux résultats  
    .select('-age') // Masquer l’âge  
    .exec(function(err, data) {  
        if (err) return console.error(err);  
        console.log('People who like burritos:', data);  
    });


    
    MONGO_URI='your_mongodb_uri_here'
