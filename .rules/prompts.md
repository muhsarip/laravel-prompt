# A Feature

## Flow
Frontend => API => Request Class / Policy => Controller => Library => Response

## Details

### File naming and directory
- Feature (example : Blog)  
    Models: Blog/Blog  
    Controller: Blog/BlogController  
    Resource : Blog/BlogResource  
    Policy : Blog/BlogPolicy  
    Request : Blog/StoreBlogRequest  
    Service : Blog/CopyBlog  

- Sub Feature (example : Comment in blog)  
    Models: Blog/Blog  
    Controller: Blog/CommentController  
    Resource : Blog/CommentResource  
    Policy : Blog/CommentPolicy  
    Request : Comment/StoreCommentRequest  
    Service : Blog/ReviewComment  

### Route Layer
- always using resource:  
  `Route::apiResource('blogs', BlogController::class)->only(['index', 'update', 'store', 'destroy']);`

- every another route in feature outside the resource, need to call first, example:
    ```
    Route::get('blogs/download', [BlogController::class, 'download'])->name('blogs.download');
    Route::apiResource('blogs', BlogController::class)->only(['index', 'update', 'store', 'destroy']);    
    ```

### Validation Layer
- always using separate validation class:  
  `app/Http/Requests/Blog/IndexBlogRequest.php`  
  `IndexBlogRequest` for index method  
  `StoreBlogRequest` for store method  
  `UpdateBlogRequest` for update method  
  `DeleteBlogRequest` for destroy method`  

- MUST implement policy based on model  
- register policy class in app/Providers/AppServiceProvider.php  
- don't forget to implement policy in validation class, example:
    ```
    class DeleteBlogRequest extends FormRequest
    {
        public function authorize()
        {
            return $this->user()->can('delete', $this->route('blog'));
        }
    ```

### Business Logic
- business logic available in controller  
- always create controller with --resource  
- always using dependency injection for model, example:  
  `public function update(UpdateBlogRequest $request, Blog $blog)`  
  so there is no need to check if the model exists or not, it automatically returns 404 if the model doesn't exist  

- if business logic is straightforward like `$blog->update([...])`, just put it in the controller method  
- if the logic is more complex, move it into separate services class

### Service Class
- use action name like "StoreBlog" "UpdateBlog","CopyBloc", naming:  
  `app/Services/Blog/StoreBlog.php` 

- the format must be:  
  `class StoreBlog { public function run($inputs, User $user) { ... } }`  
  Yes, must use `run` method, single responsibility

- the usage in controller must (less code):
    ```
    public function store(StoreBlogRequest $request, StoreBlog $action)
    {
        $action->run(blabla)
    }
    ```

### Response
- every model returned from controller must have resource file, the dir respects to dir of the model, resource file need to create with parent, example Blog => Blog/BlogResource, also the model  

- every collection must use spatie query builder for filter and sort:  
    ```php
    $tasks = QueryBuilder::for(Task::withCommonRelations())
        ->with(['section', 'recurringTask', 'pendingApprovals.assigneeTeamUser.user'])
        ->allowedFilters([
            'name',
            AllowedFilter::exact('section_id'),
            AllowedFilter::scope('due_this_week'),
        ])
        ->where('project_id', $request->project_id)
        ->withCount('attachments')
        ->withCount('comments')
        ->defaultSort('order')
        ->paginate($request->item_per_page);

    return TaskResource::collection($tasks);
    ```

- every response method needs to return object model for update, store, show  
- except destroy, it returns `response()->noContent()`

### Reporting
- this report feature, just created when it asked  
- the controller name is "download"  
    ```
    public function download(DownloadReportBlogRequest $request)
    {
        $today = date("Y-m-d");

        return (new BlogExport($request))->download("blog-report-{$today}.xlsx");
    }
    ```

- the format must be same : .code/rules/report-format.md

### Migration
- when working foreign id, do not using ->foreignId() instead using only bigInteger()

### Testing
- every endpoint API needs unit testing  
- when import the model in factory class, please be aware about the model directory

### Model
- Every model must contain fillable  
- Every model should define relation clearly

## Warning
When you import any model class in any file, please find out the correct directory, do not make any assumption