You are a Laravel API development expert.

Based on the specifications below, help me enforce consistent architectural patterns for Laravel feature modules, ensuring clarity, maintainability, and best practices.

# Objective
Design a standardized structure for implementing API-based Laravel features with clean separation of concerns: frontend interaction, validation, authorization, business logic, and response handling.

---

## 📦 Feature Flow

Frontend ➡️ API ➡️ Request Class / Policy ➡️ Controller ➡️ Service ➡️ Resource Response

---

## 📁 File Naming & Directory Convention

- **Feature** (e.g. Blog)
  - Model: `Blog/Blog`
  - Controller: `Blog/BlogController`
  - Resource: `Blog/BlogResource`
  - Policy: `Blog/BlogPolicy`
  - Request: `Blog/StoreBlogRequest`
  - Service: `Blog/CopyBlog`

- **Sub-feature** (e.g. Comment inside Blog)
  - Controller: `Blog/CommentController`
  - Resource: `Blog/CommentResource`
  - Policy: `Blog/CommentPolicy`
  - Request: `Comment/StoreCommentRequest`
  - Service: `Blog/ReviewComment`

---

## 🧭 Route Layer

- Use `apiResource`:
  ```php
  Route::apiResource('blogs', BlogController::class)->only(['index', 'update', 'store', 'destroy']);
  ```

- Define custom routes **before** the resource route:
  ```php
  Route::get('blogs/download', [BlogController::class, 'download'])->name('blogs.download');
  Route::apiResource('blogs', BlogController::class)->only(['index', 'update', 'store', 'destroy']);
  ```

---

## ✅ Validation Layer

- Use distinct Request classes:
  - `IndexBlogRequest`
  - `StoreBlogRequest`
  - `UpdateBlogRequest`
  - `DeleteBlogRequest`

- Directory: `app/Http/Requests/Blog/...`

- Authorization MUST use policy logic:
  ```php
  public function authorize()
  {
      return $this->user()->can('delete', $this->route('blog'));
  }
  ```

- Register policies in `AppServiceProvider`.

---

## 🧠 Business Logic Layer

- Keep simple logic in controller:
  ```php
  $blog->update([...]);
  ```

- Move complex logic into Service classes.

- Use dependency injection for models to auto-404 if not found:
  ```php
  public function update(UpdateBlogRequest $request, Blog $blog)
  ```

---

## ⚙️ Service Class Structure

- Naming: `app/Services/Blog/StoreBlog.php`
- Convention:
  ```php
  class StoreBlog {
      public function run($inputs, User $user) {
          ...
      }
  }
  ```

- Usage in controller:
  ```php
  public function store(StoreBlogRequest $request, StoreBlog $action)
  {
      return $action->run([...], $request->user());
  }
  ```

---

## 📤 Response Layer

- Use Resource class for all model responses.
- Resource path must reflect model path: e.g. `Blog/BlogResource`.
- Use Spatie Query Builder for collections:
  ```php
  $tasks = QueryBuilder::for(Task::withCommonRelations())
      ->with(['section', 'recurringTask', 'pendingApprovals.assigneeTeamUser.user'])
      ->allowedFilters([
          'name',
          AllowedFilter::exact('section_id'),
          AllowedFilter::scope('due_this_week'),
      ])
      ->where('project_id', $request->project_id)
      ->withCount(['attachments', 'comments'])
      ->defaultSort('order')
      ->paginate($request->item_per_page);

  return TaskResource::collection($tasks);
  ```

- `store`, `update`, and `show` → must return Resource  
- `destroy` → must return `response()->noContent()`

---

## 📈 Reporting Layer

- Only create report endpoint if requested.
- Use controller method: `download`
  ```php
  public function download(DownloadReportBlogRequest $request)
  {
      $today = date("Y-m-d");
      return (new BlogExport($request))->download("blog-report-{$today}.xlsx");
  }
  ```

- Follow format spec: `.code/rules/report-format.md`

---

## 🛠️ Migration Layer

- Avoid `->foreignId()`. Use `bigInteger()` for foreign keys.

---

## 🧪 Testing Layer

- Write unit tests for every endpoint.
- Always verify model paths in factory imports.

---

## 🧱 Model Layer

- All models must define `$fillable`.
- All relationships must be explicitly defined.

---

## ⚠️ Warning

When importing any model class, verify the **exact path**.  
**Never assume** locations or use auto-import blindly.