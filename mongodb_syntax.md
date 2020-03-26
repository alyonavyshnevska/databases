### Start 
- `mongo` to start mongodb in shell

#### Commands

- insert

``` 
db.inventory.insertMany([
   { item: "journal", qty: 25, size: { h: 14, w: 21, uom: "cm" }, status: "A" },
   { item: "notebook", qty: 50, size: { h: 8.5, w: 11, uom: "in" }, status: "A" }
])
```

- select

- `db.inventory.find( {} )` corresponds to `SELECT * FROM inventory` 

    - selects from the inventory collection all documents where the status equals "D":  
    
    `db.inventory.find( { status: "D" } )`
