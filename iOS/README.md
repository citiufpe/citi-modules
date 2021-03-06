## Modulos iOS

**Autor**: Eduardo Leite

**Contato**: eduardo.leite@citi.ufpe.br

**Git**: eduardolsneto2

1. Get e Post Request:

    __GET__: Usando Closures (Recomendado)

    Adicione na sua Classe de comunicação com o servidor(nesse exemplo será 'ServerCom'):

    ```swift
    class func httpRequest(urlString:String, callback:@escaping (_ result: [String:Any],_ error: NSError?) -> ()){
        let url = NSURL(string: urlString)
        let task = URLSession.shared.dataTask(with: url! as URL) {(data, response, error) in
            do{
                if let data = data,
                    let json = try JSONSerialization.jsonObject(with: data) as? [String:Any]{
                    callback(json,error as NSError?)
                }
            } catch {
                print("Error deserializing JSON: \(error)")
            }
        }//finishing the http request
        task.resume()
    }
    ```

    Uso da função:

    ```swift
    let url = "url do request"
    ServerCom.httpRequest(urlString:url,callback: {(result:[String:Any], error: NSError?) -> () in
        // faça o que quizer com o retorn da request (result)
    })
    ```
    
    __POST__ (by [rvlb-19](https://github.com/rvlb-19)):
    
    ```swift
    // Create an array of URLQueryItem that represent the key-value pairs of
    // the query.
    var queryItems = [URLQueryItem]()
    queryItems.append(URLQueryItem(name: "key", value: "value"))

    // Build the query with the data previously set
    var query = NSURLComponents()
    query.queryItems = queryItems

    // Create an URLRequest using a previously defined url and set its method
    // as POST
    var request = URLRequest(url: url)
    request.method = "POST"
    // Set the request body as the data set in query
    request.httpBody = query.query!.data(using: .utf8)

    // Send request (you can also put this inside a var if you wish)
    URLSession.shared.dataTask(with: request) { (data, response, error) in
        // Do stuff the same way as in GET
    }.resume()
    ```
    
    caso o post necessite de header incremente:
    
    ```swift
    request.setValue("valor", forHTTPHeaderField: "Nome da header")
    ```
    
    para algo mais complexo recomendo usar [Alamofire](https://github.com/Alamofire/Alamofire).
    
2. UISearchBar
    
    __TableView__:
    
    ```swift
    // Criando a searchbar
    searchbar = UISearchBar.init(frame: CGRect.init(x: 0.0, y: 0.0, width: self.view.frame.width, height: 44.0))
    searchbar.placeholder = "Buscar"
    // Adicionando a searchbar ao topo da tableView
    myTableView.tableheaderView = searchbar
    ```
    
    para adicionar como offset(aparecer apenas quando o usuario der scroll down):
    
    ```swift
    myTableView.setContentOffset(CGPoint.init(x: 0, y: 44), animated: true)
    ```
   
3. Storyboard

    __Mudar de Storyboard__:
    
    ```swift
    //inicia a storyboard
    let storyboard = UIStoryboard(name: "Nome da Storyboard", bundle: nil)
    //inicia a view controller inicial da storyboard
    let vc = storyboard.instantiateInitialViewController()
    //adiciona a view controller a navigation controller
    self.navigationController?.pushViewController(vc!, animated: true)
    ```
