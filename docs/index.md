---
hide:
  - title
  - navigation
  - toc
---

<style>
.profile-container {
  display: flex;
  flex-wrap: wrap;
  align-items: flex-start;
  gap: 2rem;
  margin: 2rem 0;
}

.profile-sidebar {
  flex: 0 0 300px;
  background-color: var(--md-default-bg-color);
  border: 1px solid var(--md-default-fg-color--lightest);
  border-radius: 12px;
  text-align: center;
  padding: 1.5rem;  /* 从 2rem 减少到 1.5rem */
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
}

.profile-avatar {
  width: 180px;
  height: 180px;
  border-radius: 50%;
  object-fit: cover;
}

.profile-name {
  margin: 0 0 0 0;  /* 从 0.5rem 减少到 0.3rem */
  font-size: 1.5rem;
  text-align: center;
  font-weight: 600;
}

.profile-description {
  font-size: 0.6rem;
  color: var(--md-default-fg-color--light);
  line-height: 1.5;  /* 从 1.6 减少到 1.5 */
  margin: 0.5rem 0;  /* 从 1rem 减少到 0.5rem */
}

.profile-quote {
  font-style: italic;
  font-size: 0.85rem;
  color: var(--md-default-fg-color--light);
  margin: 1rem 0 0.5rem 0;  /* 调整上下间距 */
  padding: 0.5rem 0;
  border-top: 1px solid var(--md-default-fg-color--lightest);
  border-bottom: 1px solid var(--md-default-fg-color--lightest);
}

.profile-links {
  display: flex;
  justify-content: center;
  font-size: 1rem;
}

.profile-links a {
  transition: transform 0.2s;
  display: inline-block;
}

.profile-links a:hover {
  transform: scale(1.2);
}

.profile-content {
  flex: 1;
  min-width: 400px;
  max-width: 800px;
}

.profile-content h3 {
  margin-top: 0;
  margin-bottom: 1rem;
  font-size: 1.4rem;
}

.profile-content p {
  line-height: 1.8;
  margin-bottom: 1rem;
}

.profile-content ul {
  line-height: 1.8;
  margin-left: 1.5rem;
  margin-top: 0;
}

.profile-content li {
  margin-left: 1.5rem;
  margin-bottom: 0.5rem;
}

.profile-content hr {
  margin: 2rem 0;
  border: none;
  border-top: 1px solid var(--md-default-fg-color--lightest);
}

.profile-content blockquote {
  margin: 1.5rem 0;
  padding-left: 1rem;
  border-left: 4px solid var(--md-primary-fg-color);
  font-style: italic;
  color: var(--md-default-fg-color--light);
}

.profile-content strong {
  font-weight: 600;
  color: var(--md-default-fg-color);
}

@media (max-width: 768px) {
  .profile-container {
    flex-direction: column;
  }

  .profile-sidebar {
    flex: 1 1 auto;
    max-width: 100%;
  }

  .profile-content {
    min-width: 100%;
  }
}
</style>

<div class="profile-container">

<div class="profile-sidebar">
  <img src="assets/image/avatar.jpg" alt="avatar" class="profile-avatar">

  <h3 class="profile-name">Yixuan Zheng</h3>

  <div class="profile-description">
    Student at <a href="https://www.pku.edu.cn" target="_blank">Peking University</a><br><br>
    Focusing on Machine Learning, LLMs, and Model Interpretability.
  </div>
    <div class="profile-links">
        <a href="https://github.com/VVKAHPM" target="_blank" title="GitHub">
      <i class="fa-brands fa-github"></i>
    </a>
        <a href="mailto:2400011003@stu.pku.edu.cn" title="Email">
      <i class="fa-solid fa-envelope"></i>
    </a>
          </div> 

</div>

<div class="profile-content">
  <h3>👋 Self-Introduction </h3>

  <p>I'm currently an undergraduate student at <a href="https://eecs.pku.edu.cn/index.htm" target="_blank">School of Electronics Engineering and Computer Science, Peking University</a>, majoring in <strong>Information and Computing Science</strong>.</p>

   <h3>🎓 Education Experience</h3>
    <li>2025.09 - now <strong>Undergraduate in Information and Computing Science</strong> | <a href="https://eecs.pku.edu.cn/index.htm" target="_blank">School of Electronics Engineering and Computer Science, Peking University</a>
    </li>
    <li>2024.09 - 2025.07 <strong>Undergraduate in Theoretical and Applied Mechanics</strong> | <a href="https://www.coe.pku.edu.cn/" target="_blank">College of Engineering, Peking University</a>
    </li>

  <h3>🏆 Awards & Honors</h3>

 <li>2024-2025 <strong>Merit Student of Peking University</strong> (北京大学三好学生)
    </li>
    <li>2024-2025 <strong>Huatai Science and Technology Scholarship</strong> (华泰证券科技奖学金)
    </li>

  <li>2021,2022 <strong>First Prize, National Olympiad in Informatics in Provinces (NOIP)</strong> (全国青少年信息学奥林匹克联赛省一等奖)
    </li>