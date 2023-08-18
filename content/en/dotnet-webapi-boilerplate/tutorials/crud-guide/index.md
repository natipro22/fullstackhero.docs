---
title: "Performing CRUD Operations"
description: "Performing CRUD Operations with fullstackhero's Web API"
lead: "Performing CRUD Operations with fullstackhero's Web API"
date: 2021-08-30T00:59:34+05:30
lastmod: 2021-11-29T00:44:06+05:30
draft: false
images: []
menu:
  dotnet-webapi-boilerplate:
    identifier: "crud-guide"
    name: "Implementing CRUD"
    parent: "tutorials"
weight: 12
toc: true
---
# Performing CRUD Operations with fullstackhero's Web API
## Add data models
```Model
namespace FSH.Starter.Domain.Catalog
{
    public enum CustomerType
    {
        Retail,
        Wholesale
    }
    public class Customer : AuditableEntity, IAggregateRoot
    {
        public string Name { get; set; } = default!;
        public CustomerType CustomerType { get; set; }
        public decimal CreditLimit { get; set; }
        public DateTime CustomerSince { get; set; }
    }
}
```
## Add to a database context
```DbSet
public DbSet<Customer> Customers => Set<Customer>();
```
## Create an entity
Next in the handler inject the IRepositoryWithEvents or IRepositor interfaces

```Handler
    public class CreateCustomerRequestHandler : IRequestHandler<CreateCustomerRequest, Guid>
    {
        private readonly IRepositoryWithEvents<Customer> _repository;
        public CreateCustomerRequestHandler(IRepositoryWithEvents<Customer> repository)
        {
            this._repository = repository;
        }

        public async Task<DefaultIdType> Handle(CreateCustomerRequest request, CancellationToken cancellationToken)
        {
            // Map to customer using mapster
            var customer = request.Adapt<Customer>();

            // add to a repository
            await _repository.AddAsync(customer, cancellationToken);

            // return the id
            return customer.Id;
        }
      }
```

