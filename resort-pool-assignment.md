# AI Engineer Interview Exercise: Resort Pool Management System

## Background

At our resort, guests often reserve pool chairs early in the morning, which prevents other guests from using them throughout the day. We are seeking to optimize the use of our iconic pool area by ensuring that all guests have fair access to poolside amenities.

## Objective

Develop an AI-powered system to monitor pool chair usage in real-time. Your system will help resort employees quickly identify which chairs are occupied and which are available, enhancing guest satisfaction and resource management.

## Technical Requirements

1.  **Computer Vision**: Utilize computer vision to analyze video footage from poolside cameras to detect occupied and unoccupied chairs.
2.  **Machine Learning**: Implement machine learning algorithms to enhance the accuracy of your occupancy detection model.
3.  **Generative AI**: Use generative AI techniques to synthetically generate training images that cover a range of scenarios (different times of the day, varying occupancy levels, different guest profiles, etc.) to robustly train your model without privacy concerns.

## Project Deliverables

1.  **System Design Document**:

-   Outline your proposed architecture, including data flow, AI models, and integration points.
-   Describe the technologies and frameworks you intend to use.

3.  **Proof of Concept (PoC)**:

-   A basic prototype demonstrating the core functionality.
-   Code should be well-documented and structured for scalability.

5.  **Testing and Validation Report**:

-   Detail the testing methodology and outcomes.
-   Include performance metrics of your models under different conditions.

7.  **Ethical Considerations and Privacy**:

-   Discuss how your system adheres to privacy laws and ethical guidelines, especially regarding video surveillance and data handling.

## Submission Instructions

-   **Product Demonstration:** Approach this task as if you are preparing a product for Sands to purchase. Be ready to demonstrate your solution using Azure, AWS, or any other platform of your choice that supports real-time or simulated data demonstrations. Showcase how your product functions in real time, highlighting its efficiency and usability.
-   **Proactive Engagement:** Providing comprehensive instructions on how Sands can independently set up and run your prototype prior to your live demonstration. This will allow us to explore the functionality and effectiveness of your solution in advance, enhancing the interactive experience of your presentation.

## Evaluation Criteria

-   **Functionality**: Accuracy of chair occupancy detection.
-   **Innovation**: Creative use of AI technologies, particularly in synthetic data generation.
-   **Code Quality**: Cleanliness, organization, and documentation of code.
-   **Scalability**: Suitability of the system for expansion to multiple pool areas or other similar applications.
-   **Compliance with Ethical Standards**: Adherence to privacy and ethical considerations.

## Pointers for Tackling the Problem

1.  **Computer Vision Model**: Start by exploring existing object detection models such as YOLO, SSD, or Faster-RCNN that can be adapted to recognize chairs and occupancy status.
2.  **Data Generation**: Look into generative models like GANs (Generative Adversarial Networks) to create diverse images of pool environments. Consider variations in lighting, crowd density, and chair designs.
3.  **Model Training and Validation**: Use both real and synthetic data to train your model. Ensure robust validation by simulating different real-world conditions.
4.  **Performance Optimization**: Consider implementing real-time processing optimizations, perhaps through edge computing solutions, to allow prompt response times.