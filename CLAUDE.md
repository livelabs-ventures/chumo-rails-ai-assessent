# Video Thumbnail Extractor - AI Assistant Context

## Project Overview
This is a Rails 7 application that extracts thumbnails from uploaded videos and displays them in a bento box layout. The app demonstrates modern Rails patterns with Active Storage, background jobs, and Tailwind CSS.

## Architecture

### Core Components
- **Video Model**: Manages video uploads and thumbnail associations using Active Storage
- **ThumbnailExtractor Service**: Handles FFmpeg operations for extracting frames
- **ThumbnailExtractionJob**: Background job for async thumbnail processing
- **Videos Controller**: RESTful controller for video CRUD operations

### Tech Stack
- Rails 7.2 with Hotwire (Turbo + Stimulus)
- Ruby 3.2.2
- SQLite3 database
- Tailwind CSS for styling
- Active Storage for file uploads
- Active Job for background processing
- FFmpeg via streamio-ffmpeg gem

## Key Patterns

### FFmpeg Integration
```ruby
# Extract thumbnail at specific percentage of video duration
movie = FFMPEG::Movie.new(video_path)
timestamp = movie.duration * 0.5  # 50% mark
movie.screenshot(output_path, seek_time: timestamp, resolution: '640x360')
```

### Bento Box Layout with Tailwind
```erb
<!-- Use grid with asymmetric cells -->
<div class="grid grid-cols-3 gap-4">
  <!-- Large main thumbnail -->
  <div class="col-span-2 row-span-2">...</div>
  <!-- Smaller thumbnails -->
  <div class="col-span-1">...</div>
</div>
```

### Active Storage Patterns
```ruby
# Attach multiple thumbnails
video.thumbnails.attach(io: File.open(path), filename: "thumb.jpg")

# Display with variants
image_tag video.thumbnails[0].variant(resize_to_fill: [640, 360])
```

## Common Tasks

### Adding Video Validations
- File size limits: `validates :file, size: { less_than: 100.megabytes }`
- Content type validation: Check for video/mp4, video/quicktime, video/x-msvideo

### Processing States
Video model should have states: pending, processing, completed, failed

### Error Handling
- Wrap FFmpeg operations in begin/rescue blocks
- Set video status to 'failed' on errors
- Log errors for debugging

### Progress Updates
- Use Action Cable for real-time updates
- Broadcast from background job
- Update UI with Turbo Streams

## Testing Approach
- Use fixture files for video uploads in tests
- Mock FFmpeg operations in unit tests
- System tests for full upload flow

## UI Components

### Status Indicators
```erb
<span class="<%= video.completed? ? 'bg-green-100' : 'bg-yellow-100' %>">
  <%= video.status %>
</span>
```

### Hover Effects
```css
group hover:scale-110 transition-transform duration-300
```

## Performance Considerations
- Process thumbnails in background jobs
- Use Active Storage variants for different sizes
- Implement pagination for video lists
- Cache thumbnail URLs

## Security
- Validate file types before processing
- Sanitize filenames
- Set max file size limits
- Use Active Storage's built-in security features