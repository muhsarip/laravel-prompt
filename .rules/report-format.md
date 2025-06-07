# Report Format Standards

## Excel Export Structure

### Sheet Name
- Use descriptive, feature-based names
- Format: `{Feature} Report` (e.g., "Blog Report", "User Report")

### Column Headers
- Use human-readable titles
- Follow Title Case formatting
- Include units where applicable (e.g., "Amount (USD)", "Date Created")

### Data Formatting
- **Dates**: Y-m-d H:i:s format
- **Currency**: Include currency symbol and 2 decimal places
- **Boolean**: Use "Yes/No" instead of 1/0
- **Status**: Use human-readable status names

### File Naming Convention
```
{feature}-report-{date}.xlsx
```

Examples:
- `blog-report-2025-06-07.xlsx`
- `user-report-2025-06-07.xlsx`
- `order-report-2025-06-07.xlsx`

## Implementation Example

```php
// In Controller
public function download(DownloadReportBlogRequest $request)
{
    $today = date("Y-m-d");
    
    return (new BlogExport($request))->download("blog-report-{$today}.xlsx");
}

// Export Class Structure
class BlogExport implements FromQuery, WithHeadings, WithMapping
{
    protected $request;
    
    public function __construct($request)
    {
        $this->request = $request;
    }
    
    public function query()
    {
        return Blog::query()
            ->with(['user', 'category'])
            ->when($this->request->start_date, function($query) {
                $query->whereDate('created_at', '>=', $this->request->start_date);
            })
            ->when($this->request->end_date, function($query) {
                $query->whereDate('created_at', '<=', $this->request->end_date);
            });
    }
    
    public function headings(): array
    {
        return [
            'ID',
            'Title',
            'Author',
            'Category',
            'Status',
            'Published Date',
            'Views Count',
            'Created At'
        ];
    }
    
    public function map($blog): array
    {
        return [
            $blog->id,
            $blog->title,
            $blog->user->name,
            $blog->category->name,
            ucfirst($blog->status),
            $blog->published_at ? $blog->published_at->format('Y-m-d H:i:s') : 'Not Published',
            $blog->views_count,
            $blog->created_at->format('Y-m-d H:i:s')
        ];
    }
}
```

## Required Packages

Add to your composer.json:
```json
{
    "require": {
        "maatwebsite/excel": "^3.1"
    }
}
```

## Request Validation

```php
class DownloadReportBlogRequest extends FormRequest
{
    public function authorize()
    {
        return $this->user()->can('download-reports', Blog::class);
    }
    
    public function rules()
    {
        return [
            'start_date' => 'nullable|date',
            'end_date' => 'nullable|date|after_or_equal:start_date',
            'status' => 'nullable|in:draft,published,archived',
            'category_id' => 'nullable|exists:categories,id'
        ];
    }
}
```

## Route Registration

```php
// Place before resource routes
Route::get('blogs/download', [BlogController::class, 'download'])->name('blogs.download');
Route::apiResource('blogs', BlogController::class)->only(['index', 'store', 'update', 'destroy']);
```
