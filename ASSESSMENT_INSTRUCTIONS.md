# Video Thumbnail Extraction Assessment

## Time Allocation: 1-2 hours

### What We're Looking For

This assessment evaluates your ability to:
1. Build a functional Rails application efficiently
2. Integrate AI tools effectively into your development workflow
3. Make sound architectural decisions
4. Create an attractive, user-friendly interface

### Step-by-Step Guide

#### 1. Initial Setup (10 minutes)
- [ ] Fork this repository
- [ ] Run `bundle install`
- [ ] Set up the database: `rails db:create db:migrate`
- [ ] Install Active Storage: `rails active_storage:install` && `rails db:migrate`
- [ ] Verify FFmpeg is installed: `ffmpeg -version`
- [ ] Start the server: `bin/dev`

#### 2. Create the Video Model (15 minutes)
- [ ] Generate the model: `rails generate model Video title:string duration:float status:integer`
- [ ] Add Active Storage associations
- [ ] Implement validations
- [ ] Set up the status enum
- [ ] Run migrations

#### 3. Build the Controller (15 minutes)
- [ ] Generate controller: `rails generate controller Videos`
- [ ] Implement CRUD actions
- [ ] Handle file uploads
- [ ] Trigger background job after create

#### 4. Create the Background Job (20 minutes)
- [ ] Generate job: `rails generate job ThumbnailExtraction`
- [ ] Implement thumbnail extraction logic
- [ ] Handle errors gracefully
- [ ] Update video status throughout process

#### 5. Build the Service Object (20 minutes)
- [ ] Create `app/services/thumbnail_extractor.rb`
- [ ] Implement FFmpeg integration
- [ ] Extract thumbnails at correct timestamps
- [ ] Attach thumbnails to video record

#### 6. Design the UI (30 minutes)
- [ ] Create video upload form
- [ ] Build the bento box layout
- [ ] Add Tailwind styling
- [ ] Implement hover effects
- [ ] Show processing status

#### 7. Testing & Polish (10 minutes)
- [ ] Test with different video formats
- [ ] Verify error handling
- [ ] Check responsive design
- [ ] Take screenshots for submission

### Common Pitfalls to Avoid

1. **FFmpeg Path Issues**: Ensure FFmpeg is in your PATH
2. **File Permissions**: Temp directory must be writable
3. **Memory Usage**: Don't load entire video into memory
4. **Background Job Queue**: Ensure jobs are actually running
5. **Missing Migrations**: Don't forget Active Storage tables

### Helpful Commands

```bash
# Check if Active Storage is set up
rails active_storage:install

# Create a background job
rails generate job ThumbnailExtraction

# Test FFmpeg
ffmpeg -i test_video.mp4 -ss 00:00:05 -vframes 1 output.jpg

# Run jobs synchronously for testing
Rails.application.config.active_job.queue_adapter = :inline
```

### Submission Checklist

Before submitting, ensure you have:
- [ ] Working video upload with file validation
- [ ] Thumbnail extraction at 5 timestamps
- [ ] Attractive bento box layout
- [ ] Processing status indicators
- [ ] Error handling for invalid files
- [ ] Completed CLAUDE.md file
- [ ] Filled out AI_USAGE_TEMPLATE.md
- [ ] Screenshots of working application
- [ ] Clean, committed code

### Evaluation Rubric

#### Exceptional (90-100%)
- All features working perfectly
- Excellent AI tool integration
- Clean, idiomatic Rails code
- Beautiful, responsive UI
- Comprehensive error handling
- Well-documented AI usage

#### Good (70-89%)
- Core features working
- Good AI tool usage
- Solid Rails patterns
- Nice looking UI
- Basic error handling
- AI usage documented

#### Acceptable (50-69%)
- Basic upload and extraction works
- Some AI tool usage
- Working but rough UI
- Limited error handling
- Minimal documentation

### Need Help?

Remember, this assessment is about demonstrating your ability to work WITH AI tools, not despite them. Use your AI assistant for:
- Debugging errors
- FFmpeg command syntax
- Tailwind CSS classes
- Rails best practices
- Testing strategies

Good luck! Show us how AI makes you a more effective Rails developer.