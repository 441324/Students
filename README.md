struct Person {
    var name: String
    var surname: String
    var rating: String
    var gender: String
}

class ViewController: UIViewController, UITableViewDataSource, UITableViewDelegate {
    
    let searchController = UISearchController(searchResultsController: nil)
    
    @IBOutlet weak var tableView: UITableView!
    var persons: [Person] = []
    var currentPersons: [Person] = []
    
    var person1 = Person(name: "Дима", surname: "Иванов", rating: "5", gender: "мужчина")
    var person2 = Person(name: "Костя", surname: "Петров", rating: "3", gender: "мужчина")
    var person3 = Person(name: "Сергей", surname: "Сидоров", rating: "4", gender: "мужчина")
    var person4 = Person(name: "Виталий", surname: "Григор", rating: "5", gender: "мужчина")
    var person5 = Person(name: "Мария", surname: "Пучкова", rating: "4", gender: "женщина")
    var person6 = Person(name: "Денис", surname: "Смолов", rating: "3", gender: "мужчина")
    var person7 = Person(name: "Лиза", surname: "Мусорова", rating: "5", gender: "женщина")
    var person8 = Person(name: "Артем", surname: "Кулаков", rating: "4", gender: "мужчина")
    var person9 = Person(name: "Витя", surname: "Сазонов", rating: "3", gender: "мужчина")
    var person10 = Person(name: "Катя", surname: "Ридкевич", rating: "5", gender: "женщина")
    var person11 = Person(name: "Толик", surname: "Климчук", rating: "4", gender: "мужчина")
    var person12 = Person(name: "Антон", surname: "Соколов", rating: "3", gender: "мужчина")
    var person13 = Person(name: "Андрей", surname: "Маслов", rating: "5", gender: "мужчина")
    var person14 = Person(name: "Саша", surname: "Стрелов", rating: "4", gender: "мужчина")
    var person15 = Person(name: "Коля", surname: "Землянов", rating: "3", gender: "мужчина")
    var person16 = Person(name: "Леонид", surname: "Ларин", rating: "5", gender: "мужчина")
    var person17 = Person(name: "Тарас", surname: "Синицин", rating: "4", gender: "мужчина")
    var person18 = Person(name: "Матвей", surname: "Каранюк", rating: "3", gender: "мужчина")
    var person19 = Person(name: "Вадим", surname: "Стародуб", rating: "5", gender: "мужчина")
    var person20 = Person(name: "Лена", surname: "Колесник", rating: "4", gender: "женщина")

    

    override func viewDidLoad() {
        super.viewDidLoad()
        persons = [person1, person2, person3, person4, person5, person6, person7, person8, person9, person10, person11, person12, person13, person14, person15, person16, person17, person18, person19, person20]
        currentPersons = persons.sorted(by: { (person1, person2) -> Bool in
            person1.surname < person2.surname
        })
        
        searchController.searchResultsUpdater = self
        searchController.obscuresBackgroundDuringPresentation = false
        searchController.searchBar.placeholder = "Search"
        navigationItem.searchController = searchController
        definesPresentationContext = true
        
        searchController.searchBar.scopeButtonTitles = ["Все", "М", "Ж"]
        searchController.searchBar.delegate = self
        
        
    }
    
    func filterContentForSearchText(_ searchText: String, gender: String = "Все") {
        var results: [Person] = []
        for person in persons {
            if "\(person.name) \(person.surname))".lowercased().contains(searchText.lowercased()) {
                if person.gender == "мужчина" && gender == "М" {
                    results.append(person)
                }
                
                if person.gender == "женщина" && gender == "Ж" {
                    results.append(person)
                }
                
                if gender == "Все" {
                    results.append(person)
                }
            }
        }
        
        currentPersons = results
        if searchText.isEmpty && gender == "Все" {
            currentPersons = persons.sorted(by: { (person1, person2) -> Bool in
                person1.surname < person2.surname
            })
        }
       
        tableView.reloadData()
    }
    
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return currentPersons.count
    }
    
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = tableView.dequeueReusableCell(withIdentifier: "StudentsIdentify", for: indexPath)
        cell.textLabel?.text = "\(currentPersons[indexPath.row].name) \(currentPersons[indexPath.row].surname)"
        return cell
    }
    
    func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
        tableView.deselectRow(at: indexPath, animated: true)
        print(currentPersons[indexPath.row])
    }
    
    func tableView(_ tableView: UITableView, didDeselectRowAt indexPath: IndexPath) {
        print(currentPersons[indexPath.row])
    }
}

extension ViewController: UISearchBarDelegate {
    func searchBar(_ searchBar: UISearchBar, selectedScopeButtonIndexDidChange selectedScope: Int) {
        filterContentForSearchText(searchBar.text!, gender: searchBar.scopeButtonTitles![selectedScope])
    }
}

extension ViewController: UISearchResultsUpdating {
    func updateSearchResults(for searchController: UISearchController) {
        let searchBar = searchController.searchBar
        let gender = searchBar.scopeButtonTitles![searchBar.selectedScopeButtonIndex]
        filterContentForSearchText(searchController.searchBar.text!, gender: gender)

    }
}
