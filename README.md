# 🚀 Laravel AI-Powered Development Standards

<p align="center">
  <img src="https://raw.githubusercontent.com/laravel/art/master/logo-lockup/5%20SVG/2%20CMYK/1%20Full%20Color/laravel-logolockup-cmyk-red.svg" width="300" alt="Laravel Logo">
  <br><br>
  <strong>Comprehensive coding standards and AI prompts for Laravel development</strong>
  <br>
  <em>Streamline your development process with consistent, AI-friendly coding conventions</em>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Laravel-12.x-FF2D20?style=for-the-badge&logo=laravel&logoColor=white" alt="Laravel 12">
  <img src="https://img.shields.io/badge/PHP-8.2+-777BB4?style=for-the-badge&logo=php&logoColor=white" alt="PHP 8.2+">
  <img src="https://img.shields.io/badge/AI--Friendly-Prompts-00D4AA?style=for-the-badge" alt="AI-Friendly">
  <img src="https://img.shields.io/badge/License-MIT-green?style=for-the-badge" alt="MIT License">
</p>

---

## 🎯 **What is This Project?**

This repository contains a **comprehensive set of Laravel coding standards and conventions** specifically designed to work seamlessly with **AI assistants** like GitHub Copilot, ChatGPT, Claude, and other AI code generators.

### **Why This Matters**

- 🤖 **AI-Optimized**: Clear, structured prompts that help AI understand your coding preferences
- 📐 **Consistent Architecture**: Standardized file organization and naming conventions
- ⚡ **Faster Development**: Reduce decision fatigue and speed up feature development
- 🔧 **Best Practices**: Industry-standard Laravel patterns and practices
- 👥 **Team Alignment**: Ensure all developers (human and AI) follow the same conventions

---

## 🏗️ **Architecture Overview**

This project follows a **feature-based architecture** with clear separation of concerns:

```
Frontend → API → Request/Policy → Controller → Service → Response
```

### **Key Principles**

- **Feature-Based Organization**: Group related files by feature, not by type
- **Clear Naming Conventions**: Predictable file and class naming
- **Service Layer Pattern**: Complex business logic in dedicated service classes
- **Policy-Based Authorization**: Consistent permission handling
- **Resource-Based Responses**: Standardized API responses
- **Comprehensive Testing**: Every endpoint covered by tests

---

## 📁 **File Organization Structure**

```
app/
├── Http/
│   ├── Controllers/
│   │   └── Blog/
│   │       ├── BlogController.php
│   │       └── CommentController.php
│   ├── Requests/
│   │   └── Blog/
│   │       ├── StoreBlogRequest.php
│   │       ├── UpdateBlogRequest.php
│   │       └── DeleteBlogRequest.php
│   └── Resources/
│       └── Blog/
│           ├── BlogResource.php
│           └── CommentResource.php
├── Models/
│   └── Blog/
│       └── Blog.php
├── Policies/
│   └── Blog/
│       └── BlogPolicy.php
└── Services/
    └── Blog/
        ├── StoreBlog.php
        ├── UpdateBlog.php
        └── CopyBlog.php
```

---

## 🤖 **AI Integration**

### **The Power of Structured Prompts**

The `.rules/prompts.md` file contains detailed instructions that help AI assistants understand:

- **Naming Conventions**: How to name files, classes, and methods
- **Code Organization**: Where to place different types of code
- **Architecture Patterns**: Which patterns to use and when
- **Best Practices**: Laravel-specific recommendations
- **Testing Requirements**: How to structure and write tests

### **Example AI Prompt Usage**

```
"Create a Blog feature following the project standards in .rules/prompts.md"
```

The AI will automatically generate:
- ✅ Properly organized file structure
- ✅ Consistent naming conventions  
- ✅ Request validation classes
- ✅ Policy-based authorization
- ✅ Service classes for complex logic
- ✅ API resource responses
- ✅ Complete test coverage

---

## 🚀 **Quick Start**

### **Prerequisites**

- PHP 8.2+
- Composer
- Laravel 12.x

### **Installation**

```bash
# Clone the repository
git clone https://github.com/yourusername/laravel-ai-standards.git
cd laravel-ai-standards

# Install dependencies
composer install

# Set up environment
cp .env.example .env
php artisan key:generate

# Run migrations
php artisan migrate

# Start development server
php artisan serve
```

### **Using the Standards**

1. **Review the standards**: Read `.rules/prompts.md` to understand the conventions
2. **Share with AI**: Include the rules in your AI assistant prompts
3. **Generate features**: Ask AI to create features following these standards
4. **Maintain consistency**: Use the standards across your entire project

---

## 📋 **Usage Examples**

### **🎯 Example Prompt for Complete Feature**

Here's exactly how to prompt AI assistants to create features following your standards:

```
Give me feature "blog", user can:
- lists
- view  
- create
- update
- delete
- copy blog (complex process)
- download report

Please follow the coding standards in .rules/prompts.md strictly. 
If the code doesn't match requirements, please re-read .rules/prompts.md 
as you might have missed important details.
```

### **🔧 What This Prompt Will Generate**

The AI will create a complete Blog feature with:

#### **📁 File Structure**
```
app/
├── Models/Blog/
│   └── Blog.php
├── Http/
│   ├── Controllers/Blog/
│   │   └── BlogController.php
│   ├── Requests/Blog/
│   │   ├── IndexBlogRequest.php
│   │   ├── StoreBlogRequest.php
│   │   ├── UpdateBlogRequest.php
│   │   ├── DeleteBlogRequest.php
│   │   └── DownloadReportBlogRequest.php
│   └── Resources/Blog/
│       └── BlogResource.php
├── Policies/Blog/
│   └── BlogPolicy.php
├── Services/Blog/
│   └── CopyBlog.php
└── Exports/
    └── BlogExport.php
```

#### **🛣️ Routes**
```php
// routes/api.php
Route::get('blogs/download', [BlogController::class, 'download'])->name('blogs.download');
Route::apiResource('blogs', BlogController::class)->only(['index', 'show', 'store', 'update', 'destroy']);
Route::post('blogs/{blog}/copy', [BlogController::class, 'copy'])->name('blogs.copy');
```

#### **🎮 Controller Methods**
- `index()` - List blogs with filtering using Spatie Query Builder
- `show()` - View single blog with proper resource response
- `store()` - Create new blog with validation and authorization
- `update()` - Update existing blog with dependency injection
- `destroy()` - Delete blog returning `noContent()` response
- `copy()` - Complex copy operation using service class
- `download()` - Excel report generation following standards

### **🚨 Error Handling Instructions**

When working with AI assistants, use these follow-up prompts:

#### **If AI Makes Errors:**
```
Continue - please fix the errors and complete the implementation.
```

#### **If Code Doesn't Match Standards:**
```
The code doesn't match the requirements. Please re-read .rules/prompts.md, 
I think you missed important details. Specifically check:
- File naming and directory structure
- Service layer implementation
- Request validation patterns
- Policy-based authorization
- Resource response formatting
```

### **🔄 Step-by-Step Feature Creation**

#### **Step 1: Basic Prompt**
```
Create a Blog feature following .rules/prompts.md with CRUD operations
```

#### **Step 2: Add Complex Features**
```
Add copy functionality and report download to the Blog feature, 
following the service layer and reporting standards
```

#### **Step 3: Complete Testing**
```
Create comprehensive tests for all Blog endpoints following the testing standards
```

### **📊 Advanced Usage Examples**

#### **For Complex Business Logic:**
```
Create a Blog feature with advanced workflow:
- Draft → Review → Published states
- Email notifications on status changes
- Scheduled publishing
- Follow .rules/prompts.md for service layer implementation
```

#### **For Sub-Features:**
```
Add Comment sub-feature to Blog following .rules/prompts.md:
- Belongs to blog and user
- CRUD operations with moderation
- Status: pending, approved, rejected
```

### **🎯 Quality Assurance Prompts**

Use these to ensure AI follows your standards:

```
Before generating code, please confirm you understand:
1. Feature-based directory organization
2. Service classes use single `run()` method
3. Request classes include policy authorization
4. Resources use Spatie Query Builder for collections
5. Foreign keys use bigInteger() not foreignId()
6. Every endpoint needs corresponding tests
```

### **💡 Pro Tips for AI Interaction**

1. **Always reference `.rules/prompts.md`** in your prompts
2. **Be specific about requirements** - list exact functionality needed
3. **Ask for complete implementations** - don't let AI skip parts
4. **Request tests and validation** - ensure comprehensive coverage
5. **Use "continue" for error recovery** - let AI fix and complete
6. **Reference the standards** when code doesn't match expectations

---

## 📚 **Key Features & Standards**

### **🎯 Request Validation**
- Separate request classes for each action (`StoreBlogRequest`, `UpdateBlogRequest`)
- Policy-based authorization in request classes
- Clear validation rules and error messages

### **🔐 Authorization**
- Policy classes for each model
- Consistent permission checking
- Gate-based access control

### **⚙️ Service Layer**
- Single responsibility principle
- Action-based naming (`StoreBlog`, `UpdateBlog`)
- Standardized `run()` method interface

### **📊 API Resources**
- Consistent response formatting
- Query builder integration with Spatie
- Standardized pagination and filtering

### **📈 Reporting**
- Excel export functionality
- Standardized report generation
- Download endpoint patterns

### **🧪 Testing**
- Complete API endpoint coverage
- Feature-based test organization
- Consistent test naming and structure

---

## 💡 **Benefits for Development Teams**

### **For Human Developers**
- 📖 Clear guidelines and conventions to follow
- 🔄 Consistent codebase across team members  
- ⚡ Faster onboarding for new team members
- 🎯 Reduced code review discussions about style

### **For AI Assistants**
- 🤖 Clear context about project preferences
- 📐 Structured prompts for consistent output
- 🎯 Reduced need for clarification questions
- ⚡ Higher quality generated code

---

## 🤝 **Contributing**

Contributions are welcome! Please feel free to submit a Pull Request. Areas where contributions are especially valuable:

- Additional coding standards and conventions
- AI prompt improvements and optimizations  
- Example implementations and use cases
- Documentation enhancements
- Test coverage improvements

### **How to Contribute**

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

---

## 📖 **Documentation**

- **[Coding Standards](.rules/prompts.md)** - Complete development guidelines
- **[API Documentation](docs/api.md)** - API endpoint documentation *(coming soon)*
- **[Deployment Guide](docs/deployment.md)** - Production deployment instructions *(coming soon)*

---

## 🌟 **Why Use This Approach?**

### **Traditional Development**
```
Developer writes code → Manual code review → Inconsistencies → Refactoring needed
```

### **AI-Powered Development with Standards**
```
Clear standards → AI generates consistent code → Faster reviews → Production ready
```

---

## 📄 **License**

This project is open-sourced software licensed under the [MIT license](LICENSE).

---

## 👤 **Author**

**Your Name**
- GitHub: [@yourusername](https://github.com/yourusername)
- LinkedIn: [Your LinkedIn](https://linkedin.com/in/yourprofile)
- Email: your.email@example.com

---

## 🙏 **Acknowledgments**

- The Laravel community for excellent framework and conventions
- AI development tools that make coding more efficient
- Open source contributors who inspire better development practices

---

<p align="center">
  <strong>⭐ Star this repository if you find it helpful!</strong>
  <br>
  <em>Help other developers discover AI-powered Laravel development standards</em>
</p>
