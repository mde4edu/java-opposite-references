In this use-case, we can demonstrate how model-based code generation can be used to address an important limitation of Java: the lack of synchronisable opposite references. For example, consider the following hand-written Java classes:

```java
class Customer {
  
  protected List<Invoice> invoices = new ArrayList<>();
  
  public List<Invoice> getInvoices() { 
    return invoices; 
  } 
}

class Invoice {
  
  protected Customer customer;
  
  public Customer getCustomer() { 
    return customer;
  }
  
  public Customer setCustomer(Customer customer) { 
    this.customer = customer;
  } 
}
```

If we wish to link a customer with an invoice, we need to maintain both ends of the association manually as follows:

```java
Customer c1 = new Customer();
Invoice i1 = new Invoice();
i1.setCustomer(c1);
c1.getInvoices().add(i1); // Sync the two references
```

If on the other hand we were to generate these classes from a higher-level specification (e.g. UML, Ecore model) we could implement this at the generator level (e.g. as the EMF code generator does). In this way, clients would only need to set one end of the association and the other end would be automatically synchronised as follows:

```java
Customer c1 = new Customer();
Invoice i1 = new Invoice();
i1.setCustomer(c1);
//c1.getInvoices().add(i1); -- We no longer need this
```
