Файл `Category.java`:
```java
package main.ormdemo;

import jakarta.persistence.*;
import java.util.List;

@Entity
public class Category {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false, unique = true)
    private String name;

    @OneToMany(mappedBy = "category", cascade = CascadeType.ALL, orphanRemoval = true)
    private List<Product> products;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public List<Product> getProducts() {
        return products;
    }

    public void setProducts(List<Product> products) {
        this.products = products;
    }
}
```

Файл `Product.java`:
```java
package main.ormdemo;

import jakarta.persistence.*;

@Entity
public class Product {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
    private Double price;

    @ManyToOne
    private Category category;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Double getPrice() {
        return price;
    }

    public void setPrice(Double price) {
        this.price = price;
    }

    public Category getCategory() {
        return category;
    }

    public void setCategory(Category category) {
        this.category = category;
    }
}
```

Файл `CategoryRepository.java`:
```java
package main.ormdemo;

import org.springframework.data.jpa.repository.JpaRepository;

public interface CategoryRepository extends JpaRepository<Category, Long> {}
```

Файл `CategoryService.java`
```java
package main.ormdemo;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import java.util.List;

@Service
public class CategoryService {
    private final CategoryRepository categoryRepository;
    private final ProductRepository productRepository;

    @Autowired
    public CategoryService(CategoryRepository categoryRepository, ProductRepository productRepository) {
        this.categoryRepository = categoryRepository;
        this.productRepository = productRepository;
    }

    public Category createCategory(String name) {
        Category category = new Category();
        category.setName(name);
        return categoryRepository.save(category);
    }

    public Product createProduct(String name, Double price, Long categoryId) {
        Category category = categoryRepository.findById(categoryId).orElseThrow();
        Product product = new Product();
        product.setName(name);
        product.setPrice(price);
        product.setCategory(category);
        return productRepository.save(product);
    }

    public List<Product> getProductsByCategory(Long categoryId) {
        return productRepository.findByCategory_Id(categoryId);
    }

    public Product updateProductCategory(Long productId, Long newCategoryId) {
        Product product = productRepository.findById(productId).orElseThrow();
        Category newCategory = categoryRepository.findById(newCategoryId).orElseThrow();
        product.setCategory(newCategory);
        return productRepository.save(product);
    }

    @Transactional
    public void deleteCategory(Long categoryId) {
        Category category = categoryRepository.findById(categoryId).orElseThrow();
        productRepository.deleteAll(category.getProducts());
        categoryRepository.delete(category);
    }
}

Файл `Tplab7Application.java`
```java
package main.ormdemo;

import com.sun.tools.javac.Main;
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Tplab7Application implements CommandLineRunner {
    private CategoryService categoryService;

    public void Main(CategoryService categoryService) {
        this.categoryService = categoryService;
    }

    public Tplab7Application(CategoryService categoryService) {
        this.categoryService = categoryService;
    }

    public static void main(String[] args) {
        SpringApplication.run(Tplab7Application.class, args);
    }

    @Override
    public void run(String... args) {
        // Пример использования
        Category electronics = categoryService.createCategory("Electronics");
        categoryService.createProduct("Laptop", 1000.0, electronics.getId());
        categoryService.createProduct("Smartphone", 500.0, electronics.getId());

        System.out.println(categoryService.getProductsByCategory(electronics.getId()));
    }
}
```
