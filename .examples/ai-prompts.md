# Example AI Prompts

Here are practical examples of how to use the coding standards with AI assistants.

## 🎯 Basic Feature Creation

### Prompt:
```
Create a complete Blog feature following the standards in .rules/prompts.md with the following requirements:
- CRUD operations (index, store, update, destroy)
- Title and content fields
- User relationship (belongs to user)
- Category relationship (belongs to category)
- Status field (draft, published, archived)
- Include all necessary validation, policies, and tests
```

### Expected Output:
- `app/Models/Blog/Blog.php`
- `app/Http/Controllers/Blog/BlogController.php`
- `app/Http/Requests/Blog/IndexBlogRequest.php`
- `app/Http/Requests/Blog/StoreBlogRequest.php`
- `app/Http/Requests/Blog/UpdateBlogRequest.php`
- `app/Http/Requests/Blog/DeleteBlogRequest.php`
- `app/Http/Resources/Blog/BlogResource.php`
- `app/Policies/Blog/BlogPolicy.php`
- `tests/Feature/Blog/BlogTest.php`

## 🔧 Adding Complex Service

### Prompt:
```
Following .rules/prompts.md standards, create a service class for copying a blog post with these requirements:
- Service name: CopyBlog
- Copy title with "(Copy)" suffix
- Reset status to draft
- Keep same author and category
- Clear view counts
- Include proper error handling
```

### Expected Output:
- `app/Services/Blog/CopyBlog.php` with `run()` method
- Controller integration
- Proper validation and authorization

## 📊 Report Generation

### Prompt:
```
Create a blog report download feature following .rules/prompts.md and .rules/report-format.md:
- Excel export with filtering by date range and status
- Include blog title, author, category, status, and metrics
- Follow the standard report naming convention
```

### Expected Output:
- `app/Http/Requests/Blog/DownloadReportBlogRequest.php`
- `app/Exports/BlogExport.php`
- Controller method with proper route placement

## 🔗 Sub-Feature Creation

### Prompt:
```
Following .rules/prompts.md, create a Comment sub-feature for the Blog feature:
- Belongs to blog and user
- CRUD operations
- Content field with validation
- Status field (pending, approved, rejected)
- Include moderation capabilities
```

### Expected Output:
- `app/Models/Blog/Comment.php`
- `app/Http/Controllers/Blog/CommentController.php`
- Request classes in `Comment/` directory
- `app/Http/Resources/Blog/CommentResource.php`
- `app/Policies/Blog/CommentPolicy.php`

## 🧪 Test Generation

### Prompt:
```
Create comprehensive API tests for the Blog feature following .rules/prompts.md standards:
- Test all CRUD endpoints
- Include authorization tests
- Test validation rules
- Include edge cases and error scenarios
```

### Expected Output:
- `tests/Feature/Blog/BlogTest.php` with all endpoint tests
- Proper test structure and naming
- Factory usage with correct model paths

## 📝 Migration Creation

### Prompt:
```
Create a migration for the blogs table following .rules/prompts.md:
- id, title, content, status, user_id, category_id
- Use bigInteger for foreign keys (not foreignId)
- Include proper indexes
```

### Expected Output:
- Migration file with correct structure
- Proper foreign key handling
- Appropriate indexes

## 🎨 Tips for Better AI Prompts

### ✅ Do:
- Always reference `.rules/prompts.md` in your prompts
- Be specific about requirements
- Mention related features or dependencies
- Ask for complete implementations
- Request tests and validation

### ❌ Don't:
- Assume AI knows your file structure
- Skip mentioning authorization requirements
- Forget to ask for proper imports
- Leave out testing requirements
- Assume standard Laravel conventions

## 🔄 Iterative Development

### Step 1: Basic Structure
```
Create the basic Blog model and migration following .rules/prompts.md
```

### Step 2: Add Controller
```
Add BlogController with CRUD operations following the established standards
```

### Step 3: Add Validation
```
Create request classes for Blog operations following .rules/prompts.md patterns
```

### Step 4: Add Authorization
```
Implement BlogPolicy with proper authorization rules
```

### Step 5: Add Tests
```
Create comprehensive tests for all Blog endpoints
```

## 🚀 Advanced Prompts

### Complex Business Logic
```
Following .rules/prompts.md, create a blog publishing workflow:
- Draft → Review → Published states
- Email notifications on status changes  
- Automatic scheduling for future publishing
- Include proper service classes and event handling
```

### Integration with External Services
```
Add image upload functionality to Blog feature following project standards:
- Use Laravel filesystem for storage
- Create image processing service
- Add validation for image types and sizes
- Include cleanup for deleted blogs
```

---

**Remember**: The key to successful AI-assisted development is providing clear, detailed prompts that reference your established standards!
