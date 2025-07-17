# Video Thumbnail Extraction Assessment
## Rails Engineer AI Integration Test (1-2 hours)

### Overview
Build a Rails application that allows users to upload a video file, automatically extracts 5 thumbnails at different timestamps, and displays them in an attractive bento box layout.

### Core Requirements

#### 1. Video Upload Feature
- Accept video file uploads (MP4, MOV, AVI - max 100MB)
- Use Active Storage for file handling
- Basic file validation (type and size)

#### 2. Thumbnail Extraction
- Extract 5 thumbnails at: 10%, 30%, 50%, 70%, and 90% of video duration
- Use FFmpeg via the streamio-ffmpeg gem
- Process extraction in a background job
- Store thumbnails using Active Storage

#### 3. Bento Box UI
- Display thumbnails in a modern, responsive bento box layout
- Use Tailwind CSS for styling
- Include hover effects and smooth transitions
- Show video metadata (duration, file size, format)

### Technical Specifications

#### Required Stack
- Rails 7+ with Hotwire
- Ruby 3.0+
- PostgreSQL
- Tailwind CSS
- Stimulus for interactivity
- FFmpeg (via streamio-ffmpeg gem)

#### Expected Project Structure
```
app/
├── models/
│   └── video.rb
├── controllers/
│   └── videos_controller.rb
├── jobs/
│   └── thumbnail_extraction_job.rb
├── views/
│   └── videos/
│       ├── index.html.erb
│       ├── show.html.erb
│       └── _bento_box.html.erb
└── services/
    └── thumbnail_extractor.rb
```

### Sample Implementation Guide

#### 1. Video Model
```ruby
class Video < ApplicationRecord
  has_one_attached :file
  has_many_attached :thumbnails
  
  validates :file, presence: true
  validate :correct_file_mime_type
  
  enum status: { pending: 0, processing: 1, completed: 2, failed: 3 }
  
  private
  
  def correct_file_mime_type
    if file.attached? && !file.content_type.in?(%w(video/mp4 video/quicktime video/x-msvideo))
      errors.add(:file, 'Must be a video file (MP4, MOV, or AVI)')
    end
  end
end
```

#### 2. Thumbnail Extraction Service
```ruby
class ThumbnailExtractor
  def initialize(video)
    @video = video
    @movie = FFMPEG::Movie.new(ActiveStorage::Blob.service.path_for(@video.file.key))
  end
  
  def extract_thumbnails
    @video.processing!
    
    positions = [0.1, 0.3, 0.5, 0.7, 0.9]
    
    positions.each_with_index do |position, index|
      timestamp = @movie.duration * position
      thumbnail_path = Rails.root.join("tmp", "thumbnail_#{@video.id}_#{index}.jpg")
      
      @movie.screenshot(
        thumbnail_path.to_s,
        seek_time: timestamp,
        resolution: '640x360'
      )
      
      @video.thumbnails.attach(
        io: File.open(thumbnail_path),
        filename: "thumbnail_#{index}.jpg",
        content_type: 'image/jpeg'
      )
      
      File.delete(thumbnail_path)
    end
    
    @video.completed!
  rescue => e
    @video.failed!
    raise e
  end
end
```

#### 3. Bento Box View Component
```erb
<!-- app/views/videos/_bento_box.html.erb -->
<div class="max-w-4xl mx-auto p-6">
  <div class="bg-white rounded-2xl shadow-xl overflow-hidden">
    <!-- Header -->
    <div class="bg-gradient-to-r from-purple-600 to-blue-600 text-white p-6">
      <h2 class="text-2xl font-bold"><%= video.file.filename %></h2>
      <div class="mt-2 flex gap-4 text-sm">
        <span>Duration: <%= format_duration(video.duration) %></span>
        <span>Size: <%= number_to_human_size(video.file.byte_size) %></span>
        <span>Format: <%= video.file.content_type.split('/').last.upcase %></span>
      </div>
    </div>
    
    <!-- Bento Grid -->
    <div class="p-6">
      <div class="grid grid-cols-3 gap-4 max-w-3xl mx-auto">
        <!-- Large thumbnail (main) -->
        <div class="col-span-2 row-span-2 group relative overflow-hidden rounded-xl shadow-lg">
          <%= image_tag video.thumbnails[2], 
              class: "w-full h-full object-cover transition-transform duration-300 group-hover:scale-110",
              loading: "lazy" %>
          <div class="absolute inset-0 bg-gradient-to-t from-black/50 to-transparent opacity-0 group-hover:opacity-100 transition-opacity duration-300">
            <span class="absolute bottom-4 left-4 text-white font-semibold">50% - Center Frame</span>
          </div>
        </div>
        
        <!-- Smaller thumbnails -->
        <% [[0, "10%"], [1, "30%"], [3, "70%"], [4, "90%"]].each do |index, label| %>
          <div class="group relative overflow-hidden rounded-lg shadow-md hover:shadow-xl transition-shadow duration-300">
            <%= image_tag video.thumbnails[index], 
                class: "w-full h-full object-cover transition-transform duration-300 group-hover:scale-110",
                loading: "lazy" %>
            <div class="absolute inset-0 bg-gradient-to-t from-black/50 to-transparent opacity-0 group-hover:opacity-100 transition-opacity duration-300">
              <span class="absolute bottom-2 left-2 text-white text-sm font-medium"><%= label %></span>
            </div>
          </div>
        <% end %>
      </div>
      
      <!-- Status indicator -->
      <div class="mt-6 text-center">
        <span class="inline-flex items-center px-4 py-2 rounded-full text-sm font-medium
          <%= video.completed? ? 'bg-green-100 text-green-800' : 'bg-yellow-100 text-yellow-800' %>">
          <% if video.processing? %>
            <svg class="animate-spin -ml-1 mr-2 h-4 w-4" fill="none" viewBox="0 0 24 24">
              <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
              <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4z"></path>
            </svg>
          <% end %>
          <%= video.status.humanize %>
        </span>
      </div>
    </div>
  </div>
</div>
```

### AI Integration Requirements

#### 1. Claude.md File
Candidates must create a `claude.md` file that includes:
- Project overview and purpose
- Tech stack details
- FFmpeg integration patterns
- Tailwind component styling guidelines
- Common tasks and their implementations

#### 2. AI Usage Documentation
Create an `AI_USAGE.md` file documenting:
- Which AI tools were used (Cursor, GitHub Copilot, Claude, etc.)
- Specific prompts that were helpful
- Any AI suggestions that were rejected and why
- Iterations needed to get working code

#### 3. Demonstration of AI Leverage
Show effective use of AI for:
- FFmpeg command construction
- Tailwind CSS bento box layout design
- Background job error handling
- Progress indication implementation

### Evaluation Criteria

#### Technical Implementation (40%)
- [ ] Working file upload with validation
- [ ] Successful thumbnail extraction at correct timestamps
- [ ] Proper background job implementation
- [ ] Error handling for failed videos

#### UI/UX Quality (30%)
- [ ] Attractive bento box layout
- [ ] Responsive design
- [ ] Smooth hover effects and transitions
- [ ] Clear status indicators

#### AI Integration (30%)
- [ ] Quality of Claude.md file
- [ ] Effective AI tool usage documentation
- [ ] Evidence of iterative AI collaboration
- [ ] Critical evaluation of AI suggestions

### Bonus Points
- Real-time progress updates using Action Cable
- Drag-and-drop file upload
- Video preview on hover
- Download all thumbnails as ZIP
- Custom timestamp selection

### Deliverables
1. Complete Rails application
2. `claude.md` file with AI instructions
3. `AI_USAGE.md` documenting your AI tool usage
4. README with setup instructions
5. Screenshots of the working bento box UI

### Time Expectation
This assessment should take 1-2 hours for a proficient Rails developer using AI tools effectively. Focus on demonstrating how AI accelerates your development rather than writing everything from scratch.

### Setup Instructions for Candidates
```bash
# Create new Rails app
rails new video-thumbs --css=tailwind --database=postgresql

# Add required gems to Gemfile
gem 'streamio-ffmpeg'
gem 'image_processing' # for Active Storage variants

# Install FFmpeg (macOS)
brew install ffmpeg

# Install FFmpeg (Ubuntu)
sudo apt update
sudo apt install ffmpeg

# Setup database
rails db:create
rails db:migrate

# Generate video scaffold
rails generate scaffold Video title:string duration:float status:integer
```

### Example Claude.md Starter
```markdown
# Video Thumbnail Extractor

This Rails app extracts thumbnails from uploaded videos and displays them in a bento box layout.

## Architecture Decisions
- Use Active Storage for file handling to simplify video and image management
- Process thumbnails in background jobs to keep UI responsive
- Implement bento box layout with Tailwind CSS grid system

## Key Patterns
- Service objects for video processing logic
- Stimulus controllers for interactive elements
- ViewComponent or partials for reusable UI components

## FFmpeg Commands
Extract thumbnail at specific timestamp:
`ffmpeg -i input.mp4 -ss 00:00:10 -vframes 1 -vf scale=640:360 output.jpg`

## Tailwind Bento Box
Use grid with col-span and row-span for asymmetric layouts:
- Main item: col-span-2 row-span-2
- Side items: col-span-1 row-span-1
```

This streamlined assessment maintains the core evaluation goals while being more achievable in a shorter timeframe. It still tests Rails proficiency, AI tool integration, and produces a visually appealing result that candidates can be proud of.