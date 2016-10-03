# knockthru

Allows you to databind the Knockout UI in the client directly against the Mongoose datamodel without having to write glue code.

# Examples

To list the Tasks in a database that have done set to 0
```
	<div data-knockthru='kt.search("Task",{done:0})'>
		<p data-bind='foreach: items'>
		...
		</p>
	</div>
```

To provide a box for the user to define a new Task
```
    <tfoot data-knockthru='kt.create("Task")' data-bind='with: item'>
        <tr>
            <td><input type="text" class="form-control" data-bind='textInput: description, onEnterKey: (description ? $parent.submitCreate : null)' placeholder='Enter Task Description' ></td>
            <td><button class='form-control' data-bind='enable: description, click:$parent.submitCreate'>Add</input></td>
        </tr>            
    </tfoot>  
```

To display a specific task identified by ?id=<some identifier> on the querystring
```
	<div data-knockthru='kt.read("Task",kt.getUrlParameter("id"))'>
		<div data-bind='with: item' >
            <div class="form-group">
                <label for="descriptionInput">Description</label>        
                <input type="text" class="form-control" id="descriptionInput" 
                    data-bind='textInput: description'>
                </div>

            <div class="checkbox">
                <label for="doneInput">
                <input type='checkbox' data-bind='checked: done' id="doneInput">Done</input>
                </label>
            </div>
        </div>
	</div>
```

# ViewModel Functions

These methods can be used in the data-knockthru attributes

## kt.search(modelname[, filters])

Generates a viewmodel the can display a readonly list of items

|element    | description                                                                            |
|-----------|----------------------------------------------------------------------------------------|
|items      | an observable array of the result of the search                                        |
|createItem | an observable 'blank' you can bind to for create functionality (see create)            |
|errors     | an observable list of strings detailing any errors / validation errors from the server |
|refresh    | event handler to re-run the query                                                      |


## kt.searchEdit(modelname[, filters])

Generates a viewmodel for an editable list of items 

|element    | description                                                                            |
|-----------|----------------------------------------------------------------------------------------|
|items      | an observable array of the result of the search                                        |
|createItem | an observable 'blank' you can bind to for create functionality (see create)            |
|errors     | an observable list of strings detailing any errors / validation errors from the server |
|isDirty    | observable boolean indicating whether changes have been made                           |
|delete     | event handler to delete an item                                                        |
|submit     | event handler to save all the changes made                                             |
|refresh    | event handler to re-run the query                                                      |

Note on delete: use within the context of one of the items array e.g. data-bind='click:$parent.delete' 
```
    <tbody data-knockthru='kt.searchEdit("Task")' data-bind='foreach: items'>
        <tr>
        ...
            <td><button class='form-control btn-xs'><span class='glyphicon glyphicon-remove' data-bind='click:$parent.delete'></button></td>
        </tr>
    </tbody>
```

## kt.create(modelname[, predicates])

Generates a viewmodel for creating a new item.  The default values of the fields are obtained from the server.

|element    | description                                                                            |
|-----------|----------------------------------------------------------------------------------------|
|blank      | an observable blank item with fields populated with default values from the server     |
|error      | an observable string detailing any errors / validation errors from the server          |
|submitCreate | event handler to write the new item to the server                                    |
|addToSearch  | event handler to add the item to a search/searchEdit viewmodel on the page           |

## kt.read(modelname, id)

Generates a viewmodel for binding to a specific item in the database, identified by the id

|element    | description                                                                            |
|-----------|----------------------------------------------------------------------------------------|
|item       | an observable item representing the object read from the server                        |
|error      | an observable string detailing any errors / validation errors from the server          |
|refresh    | event handler to re-run the query                                                      |
|submitUpdate | event handler to write the new item to the server                                    |
|submitDelete | event handler to write the new item to the server                                    |

