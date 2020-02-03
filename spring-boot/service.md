# Service

```java
package com.example.demo.course;

import java.util.ArrayList;
import java.util.List;

import com.example.demo.topic.Topic;
import com.example.demo.topic.TopicRepository;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class CourseService {

    private CourseRepository courseRepository;
    private TopicRepository topicRepository;

    @Autowired
    public CourseService(CourseRepository courseRepository, TopicRepository topicRepository) {
        this.courseRepository = courseRepository;
        this.topicRepository = topicRepository;
    }

    public void addCourse(Course course, String topicId) {
        Topic courseTopic = topicRepository.findById(topicId).get();
        course.setTopic(courseTopic);
        courseRepository.save(course);
    }

    public void deleteCourse(String id) {
        courseRepository.deleteById(id);
    }

    public List<Course> getAllCourses(String topicId) {
        List<Course> topics = new ArrayList<>();
        courseRepository.findByTopicId(topicId).forEach(topics::add);
        return topics;
    }

    public Course getCourse(String id) {
        return courseRepository.findById(id).get();
    }

    public void updateCourse(String id, Course topic) {
        courseRepository.save(topic);
    }
}
```

