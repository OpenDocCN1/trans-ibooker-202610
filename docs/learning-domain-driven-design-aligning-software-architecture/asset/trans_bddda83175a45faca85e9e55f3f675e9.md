

## 第15章
+   1. D：A 和 B 正确。
+   2. B：事件携带状态传输。
+   3. A：开放主机服务。
+   4. B：S2 应发布公共事件通知，这将触发 S1 发出同步请求以获取最新信息。

## 第16章
+   1. D：A 和 C 正确。
+   2. B：开放主机服务。开放主机服务所公开的其中一种语言可以是针对分析处理的 OLAP 数据。
+   3. C：CQRS。CQRS 模式可用于从事务模型生成 OLAP 模型的投影。
+   4. A：有界上下文。

## 参考文献
+   - Brandolini, A. (n.d.). *Introducing EventStorming*. Leanpub.
+   - Brooks, F. P., Jr. (1974). *The Mythical Man Month and Other Essays on Software Engineering*. Reading, MA: Addison-Wesley.
+   - Eisenhardt, K., & Sull, D. (2016). *Simple Rules: How to Succeed in a Complex World*. London: John Murray.
+   - Esposito, D., & Saltarello, A. (2008). *Architecting Applications for the Enterprise: Microsoft® .NET*. Redmond, WA: Microsoft Press.
+   - Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Boston: Addison-Wesley.
+   - Feathers, M. C. (2005). *Working Effectively with Legacy Code*. Upper Saddle River, NJ: Prentice Hall PTR.
+   - Fowler, M. (2002). *Patterns of Enterprise Application Architecture*. Boston: Addison-Wesley.
+   - Fowler, M. (2019). *Refactoring: Improving the Design of Existing Code* (2nd ed.). Boston: Addison-Wesley.
+   - Fowler, M. (n.d.). *What do you mean by “Event-Driven”?* Retrieved August 12, 2021, from https://martinfowler.com/articles/201701-event-driven.html.
+   - Gamma, E., Helm, R., & Johnson, R. (1994). *Design Patterns: Elements of Reusable Object-Oriented Software*. Reading, MA: Addison-Wesley.
+   - Gigerenzer, G., Todd, P. M., & ABC Research Group (Research Group, Max Planck Institute, Germany). (1999). *Simple Heuristics That Make Us Smart*. New York: Oxford University Press.
+   - Goldratt, E. M. (2005). *Beyond the Goal: Theory of Constraints*. New York: Gildan Audio.
+   - Goldratt, E. M., & Goldratt-Ashlag, E. (2018). *The Choice*. Great Barrington, MA: North River Press Publishing Corporation.
+   - Goldratt-Ashlag, E. (2010). “The Layers of Resistance—The Buy-In Process According to TOC.” (Chapter 20 of the *Theory of Constraints* handbook.) Bedford, England: Goldratt Marketing Group.
+   - Garcia-Molina, H., & Salem K. (1987). *Sagas*. Princeton, NJ: Department of Computer Science, Princeton University.
+   - Helland, P. (2020). Data on the outside versus data on the inside. *Communications of the ACM*, 63(11), 111–118.
+   - Hohpe, G., & Woolf, B. (2003). *Enterprise Integration Patterns: Designing, Building, and Deploying Messaging Solutions*. Boston: Addison-Wesley.
+   - Khonononov, V. (2022). *Balancing Coupling in Software Design*. Boston: Addison-Wesley.
+   - Khonononov, V. (2019). *What Is Domain-Driven Design?* Boston: O'Reilly.
+   - Martaire, C. (2019). *Living Documentation: Continuous Knowledge Sharing by Design*. Boston: Addison-Wesley.
+   - Millett, S., & Tune, N. (2015). *Patterns, Principles, and Practices of Domain-Driven Design* (1st ed.). Nashville: John Wiley & Sons.
+   - Myers, G. J. (1978). *Composite/Structured Design*. New York: Van Nostrand Reinhold.
+   - Ousterhout, J. (2018). *A Philosophy of Software Design*. Palo Alto, CA: Yaknyam Press.
+   - Richardson, C. (2019). *Microservice Patterns: With Examples in Java*. New York: Manning Publications.
+   - Vernon, V. (2013). *Implementing Domain-Driven Design*. Boston: Addison-Wesley.
+   - Vernon, V. (2016). *Domain-Driven Design Distilled*. Boston: Addison-Wesley.
+   - West, G. (2018). *Scale: The Universal Laws of Life and Death in Organisms, Cities and Companies*. Oxford, England: Weidenfeld & Nicolson.
+   - Wright, D., & Meadows, D. H. (2009). *Thinking in Systems: A Primer*. London: Earthscan.

## 索引

A

accidental complexity
  case study of applying DDD, The real problem
  growth and, Growth, Bounded Contexts, Aggregates

active records, Active Record-When to Use Active Record
  design evolving
    active record to domain model, Active Record to Domain Model
    transaction script to active record, Transaction Script to Active Record
  design heuristics, Business Logic Implementation Patterns, Architectural Patterns
  Fowler naming, When to Use Active Record
  implementation, Implementation
  layered architecture, When to Use Layered Architecture
  when to use, When to Use Active Record

aggregates, Aggregates-Ubiquitous language
  about, Hierarchy of entities
    boundaries, Aggregates
  case study of applying DDD

customer relationship management bounded context, Bounded Context #2: CRM-A broader understanding of domain-driven design

marketing bounded context, Bounded Context #1: Marketing

commands, Consistency enforcement

communication

aggregate root, The aggregate root

domain events, The aggregate root, Integrating Aggregates -Process Manager

outbox pattern, Outbox

process manager, Process Manager-Process Manager

saga pattern, Saga-Consistency

complexity reduction, Managing Complexity

consistency and aggregate boundaries, Consistency

consistency enforcement, Consistency enforcement-Consistency enforcement

domain services coordinating, Domain services

“Effective Aggregate Design” (Vernon), More “aggregates”!

entities, Aggregates

identification field, Aggregates, Deleting Data

EventStorming, Step 9: Aggregates

growth management, Aggregates

hierarchy of entities, Hierarchy of entities-Hierarchy of entities

microservices, Aggregates
referencing other aggregates, Referencing other aggregates
state persisted versus event sourcing, Modeling the Dimension of Time, Event-Sourced Domain Model
(see also domain model; event-sourced domain model)
Ticket aggregate (see help desk)
transaction boundary, Transaction boundary
domain services and, Domain services
ubiquitous language, Ubiquitous language
Agile Manifesto, Tools
analysis model, Communication
analytical data management platforms
about, Analytical Data Management Platforms
analytical versus transactional model, Analytical Data Model Versus Transactional Data Model
data lake, Data Lake-Data Lake
challenges of, Challenges of Data Warehouse and Data Lake Architectures
data mesh
about, Data Mesh
data as product, Data as a Product, Enable Autonomy
decomposing data around domains, Decompose Data Around Domains

domain-driven design and, Combining Data Mesh and Domain-Driven Design

governance ecosystem, Build an Ecosystem

data warehouse, Data Warehouse-Data Warehouse

challenges of, Challenges of Data Warehouse and Data Lake Architectures

analyzing business domains (see domain analysis)

anemic domain model antipattern, When to Use Active Record, Bounded Context #1: Marketing

(see also active records)

answers to exercise questions, Chapter 1-Chapter 16

anticorruption layer, Anticorruption Layer

microservices, Anticorruption Layer

model translation, Model Translation-Asynchronous

modernization strategy, Strategic Modernization

APIs in application layer, Consistency enforcement

application layer, Consistency enforcement, Dependency Inversion Principle

(see also service layer)

applying domain-driven design (see case study of applying DDD)

architecture (see software architecture)

audit log

event sourcing for, Advantages

text file instead, Why Can’t I Just…?

monetary transactions, Business Logic Implementation Patterns

# B

backend-for-frontend pattern, Unifying multiple sources

big ball of mud, Growth

(see also real-world domain-driven design)

distributing via events, Distributed Big Ball of Mud

The Blue Book (Evans), Foreword

book source code, Using Code Examples

book website, Using Code Examples

boundaries

about, Boundaries, Integrating Bounded Contexts

aggregates, Aggregates

case study discussion, Boundaries of Bounded Contexts

event-driven architecture, Designing Event-Driven Integration

growth management, Subdomains

logical boundaries, Physical Boundaries

layers in layered architecture, Layered Architecture

service layer as, Service layer

microservices

about, Domain-Driven Design and Microservices’ Boundaries

aggregates, Aggregates

