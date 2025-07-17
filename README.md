# Video Thumbnail Extraction Assessment - Rails Engineer AI Integration Test

Welcome! This assessment is designed to evaluate your ability to build a Rails application while effectively leveraging AI tools. You should complete this in 1-2 hours.

## 🎯 Your Mission

Build a Rails application that:
1. Allows users to upload video files
2. Automatically extracts 5 thumbnails at different timestamps
3. Displays them in an attractive bento box layout

## 🚀 Getting Started

### Prerequisites
- Ruby 3.0+
- Rails 7+
- FFmpeg installed on your system
- Your favorite AI coding assistant (Cursor, GitHub Copilot, Claude, etc.)

### Install FFmpeg

**macOS:**
```bash
brew install ffmpeg
```

**Ubuntu/Debian:**
```bash
sudo apt update
sudo apt install ffmpeg
```

**Windows:**
Download from [FFmpeg official site](https://ffmpeg.org/download.html)

### Setup Instructions

1. Fork this repository
2. Install dependencies:
   ```bash
   bundle install
   ```
3. Add required gems to your Gemfile:
   ```ruby
   gem 'streamio-ffmpeg'
   gem 'image_processing', '~> 1.2'
   ```
4. Run bundle install again:
   ```bash
   bundle install
   ```
5. Create and migrate the database:
   ```bash
   rails db:create
   rails db:migrate
   ```
6. Start the Rails server:
   ```bash
   bin/dev
   ```

## 📋 Requirements

### Core Features

1. **Video Upload Feature**
   - Accept video file uploads (MP4, MOV, AVI - max 100MB)
   - Use Active Storage for file handling
   - Basic file validation (type and size)

2. **Thumbnail Extraction**
   - Extract 5 thumbnails at: 10%, 30%, 50%, 70%, and 90% of video duration
   - Use FFmpeg via the streamio-ffmpeg gem
   - Process extraction in a background job
   - Store thumbnails using Active Storage

3. **Bento Box UI**
   - Display thumbnails in a modern, responsive bento box layout
   - Use Tailwind CSS for styling (already installed)
   - Include hover effects and smooth transitions
   - Show video metadata (duration, file size, format)

### Expected Project Structure

You'll need to create:
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

## 🤖 AI Integration Requirements

### 1. Create a CLAUDE.md File
Document your project for AI assistants including:
- Project overview and architecture
- Tech stack details
- FFmpeg integration patterns
- Tailwind component styling guidelines
- Common tasks and their implementations

### 2. Create an AI_USAGE.md File
Document your AI tool usage:
- Which AI tools you used
- Specific prompts that were helpful
- Any AI suggestions you rejected and why
- Iterations needed to get working code

### 3. Demonstrate Effective AI Usage
Show how you leveraged AI for:
- FFmpeg command construction
- Tailwind CSS bento box layout design
- Background job error handling
- Progress indication implementation

## 📊 Evaluation Criteria

### Technical Implementation (40%)
- [ ] Working file upload with validation
- [ ] Successful thumbnail extraction at correct timestamps
- [ ] Proper background job implementation
- [ ] Error handling for failed videos

### UI/UX Quality (30%)
- [ ] Attractive bento box layout
- [ ] Responsive design
- [ ] Smooth hover effects and transitions
- [ ] Clear status indicators

### AI Integration (30%)
- [ ] Quality of CLAUDE.md file
- [ ] Effective AI tool usage documentation
- [ ] Evidence of iterative AI collaboration
- [ ] Critical evaluation of AI suggestions

## 🌟 Bonus Points
- Real-time progress updates using Action Cable
- Drag-and-drop file upload
- Video preview on hover
- Download all thumbnails as ZIP
- Custom timestamp selection

## 📦 Deliverables

1. Complete Rails application (push to your forked repo)
2. `CLAUDE.md` file with AI instructions
3. `AI_USAGE.md` documenting your AI tool usage
4. Screenshots of the working bento box UI
5. Any additional notes about your approach

## 💡 Tips

- Focus on demonstrating AI-assisted development rather than writing everything from scratch
- Use AI to help with FFmpeg commands - they can be tricky!
- Leverage AI for Tailwind CSS layouts - bento boxes are perfect for this
- Document your thought process and AI interactions
- Don't forget to handle edge cases (invalid files, processing failures)

## 🛠️ Helpful Resources

- [Active Storage Guide](https://guides.rubyonrails.org/active_storage_overview.html)
- [Active Job Guide](https://guides.rubyonrails.org/active_job_basics.html)
- [Streamio FFMPEG Documentation](https://github.com/streamio/streamio-ffmpeg)
- [Tailwind CSS Grid](https://tailwindcss.com/docs/grid-template-columns)

## 🚦 Getting Started Checklist

- [ ] Fork this repository
- [ ] Install FFmpeg on your system
- [ ] Add required gems to Gemfile
- [ ] Set up database
- [ ] Create your CLAUDE.md file
- [ ] Start building with your AI assistant!

Good luck! We're excited to see how you leverage AI to build this application efficiently.