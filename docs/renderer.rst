.. _renderer:


.. Start with intro to synfig cli and how it starts rendering
   Introduce targets, null, tile and scanline
   Difference between target and surface
   Use Target_Scanline as base
   Explain Target_Scanline::render
   Explain Target_Scaline::call_renderer
   Explain Canvas::build_rendering_task, reference to Task page
   Now start with Renderer::run
   Introduce optimizers and render queue
   Explain render queue
   Explain how render engines are choosen

Renderer Docs
=====================

This section explains the different parts of Cobra Engine and the Algorithm which uses them to render images.

.. toctree::
   :maxdepth: 1
   :glob:

   renderer_docs/introduction
   renderer_docs/target_surface
   renderer_docs/tasks
   renderer_docs/render_queue
   renderer_docs/optimizers
