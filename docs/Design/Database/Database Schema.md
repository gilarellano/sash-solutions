>[!warning] Work In Progress
 
>[!todo]+ To Do
> - Add more attributes to QuoteDetails
> - Create Graph
> - Finalize and Peer Review w/ a Professor

PostgreSQL or MySQL for relational data storage

**Users**:
    - `userID` (Primary Key)
    - `username`
    - `password` (hashed and salted)
    - `role` (e.g., admin, sales team)
    - `location` (if different locations have different sales teams)
    - `email`
    - `lastLogin`

**Customers**:
    - `customerID` (Primary Key)
    - `name`
    - `email`
    - `phoneNumber`
    - `projectAddress`

 **Projects**:
    - `projectID` (Primary Key)
    - `customerID` (Foreign Key)
    - `projectName`
    - `projectAddress`
    - `startDate`
    - `endDate`
    - `status` (e.g., ongoing, completed)

**Quotes** (Expanded):
    - `quoteID` (Primary Key)
    - `projectID` (Foreign Key)
    - `userID` (Foreign Key)
    - `dateCreated`
    - `status`
    - `subtotal`
    - `tax`
    - `total`
    - `discountID` (Foreign Key)

**QuoteDetails**:
    - `detailID` (Primary Key)
    - `quoteID` (Foreign Key)
    - `windowType` (e.g., single hung, casement)
    - `windowWood` (e.g., Douglas Fir, Mahogany)
    - `windowGlass` (e.g., SP, DP Low-E)
    - `unitCost`
    - `unitTax`
    - `finish` (e.g., painted, primer)
    - `glazing` (e.g., single pane, dual pane)
    - `specialDesign` (e.g., arch, true divided lites)
    - ... (other details as required)

**Pricing**:
    - `priceID` (Primary Key)
    - `item` (e.g., DOUGLAS_FIR_COST_FT, SP_COST_SQFT)
    - `price`
    - `lastUpdated`

**Discounts & Promotions**:
    - `discountID` (Primary Key)
    - `description`
    - `amount` (either a fixed amount or percentage)
    - `startDate`
    - `endDate`

**Attachments**:
    - `attachmentID` (Primary Key)
    - `quoteID` (Foreign Key)
    - `filePath`
    - `fileType`
    - `dateUploaded`

### Relationships:

**Users to Quotes**:
    - **One-to-Many**: One user can generate multiple quotes, but each quote is associated with only one user.

**Customers to Quotes**:
    - **One-to-Many**: One customer can have multiple quotes, but each quote is for one customer.

**Quotes to Pricing**:
    - **Many-to-Many**: A quote can have multiple items with different prices, and each pricing item can be associated with multiple quotes. This relationship would typically be represented using a junction table (e.g., `QuotePricing`).

**Quotes to Attachments**:
    - **One-to-Many**: One quote can have multiple attachments, but each attachment is associated with only one quote.

**Quotes to Discounts & Promotions**:
    - **Many-to-One**: Each quote can have one discount applied, but each discount can be applied to multiple quotes.

**Customers to Projects**:
    - **One-to-Many**: One customer can have multiple projects, but each project is associated with only one customer.

**Projects to Quotes**:
    - **One-to-Many**: One project can have multiple quotes (iterations, adjustments), but each quote is associated with only one project.

**Quotes to QuoteDetails**:
    - **One-to-Many**: One quote can have multiple detailed line items (each window type, finish, etc.), but each line item is associated with only one quote.

### Additional Notes:

- **Normalization**: The schema is designed to be normalized, reducing data redundancy and ensuring data integrity. For instance, instead of storing pricing directly in the `Quotes` table, we reference the `Pricing` table. This way, if a price changes, you only need to update it in one place.

- **Indexes**: To optimize query performance, consider adding indexes to frequently queried columns, especially foreign keys.

- **Constraints**: Add constraints to ensure data integrity. For example, the `status` column in the `Quotes` table might be constrained to only allow specific values (e.g., 'pending', 'approved', 'sent').

- **Quote Iterations**: By having a separate `Quotes` table linked to `Projects`, you can easily manage multiple iterations or adjustments of quotes for a single project. Each new iteration would be a new entry in the `Quotes` table associated with the same project.

- **Detailed Breakdown**: The `QuoteDetails` table allows for a granular breakdown of each quote. This is especially useful for detailed invoicing, inventory management, and analytics.

- **Normalization**: This expanded schema continues to follow normalization principles. For instance, instead of storing every window detail directly in the `Quotes` table, we have a separate `QuoteDetails` table. This allows for more flexibility and a cleaner data structure.

- **Performance**: As the database grows, especially the `QuoteDetails` table, it's essential to monitor performance. Proper indexing and occasional database optimization will be crucial.