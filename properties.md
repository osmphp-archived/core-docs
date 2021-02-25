# Properties

{{ toc }}

## Dependencies

But let's start from the beginning. PHP objects rarely operate in a vacuum, they use other PHP objects called **services**.

For instance, if some class needs to log something into a file, it won't handle it all by itself. Instead, it will use a logger service obtained from the global service container, `$osm_app`:

    /**
     * @property Logger $logger
     */
    class A {
        protected function get_logger(): Logger {
            global $osm_app; /* @var App $osm_app*/

            return $osm_app->logger;
        }

        public function x(): void {
            $this->logger->log('Starting ...');            
        }
    }


In real-life applications, hundreds or thousands of classes use other several other classes each, and to handle all that, the library uses

1. Service container